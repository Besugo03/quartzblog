I'm gonna spoil that there isn't lots of coding in this devlog, I'm mainly gonna go through my reasoning process for the implementation of the queueing system since it has been bugging me for a while with all the possible solutions there are.

---

Here's the thing : now that i want to re-make the queueing system, i have lots of choices :
- do i write basically **the same extension** for Automatic1111?
- do i make an **external script** that uses the stable diffusion / Automatic1111 API?

since I have no clue on how to make an extension for stable diffusion, and since inu-diffusion will be a standalone software anyways, I figured the latter one would make more choice, since it would also make me "independent" on whatever UI it uses, which also means **less stuff to mantain**.

Okay, now that we figured this out, we have these other choices :
- Would the queue be handled on the **client** or on the **server**?
- What **language** to write it in?

the two are pretty tied together IMO. if it goes on client, for which I'm planning to use **React/Electron**, it would make sense to integrate it directly into it, no external stuff or calls required.

if it's on the backend, it would make sense to either make it in Python for continuity's sake, or even in Go since I discovered it has a really good implementation for First-In-First-Out queues and parallelism.

Let's try and analyse how the normal txt2img API works in Automatic1111.

i made the `test_txt2img` function which is basically a carbon copy of the old txt2img function for Agent Scheduler, since it used basically the same format for the request.

Let's try calling it with a test prompt :
![[Pasted image 20241120211917.png]]

the code gets stuck for a good while. At least until the generation batch finishes. once it's done, I got this response :
```json
{'images': ['iVBORw0KGgoAAAANSUhEUgAABAAAAAQACAIAAADwf7zUAAAETnRFWHRwYXJhbWV
// ........
]}
```
actual image here :
<div style="display:flex; justify-content:center">

![[00000-4224484739.png|300]]

</div>

this time, instead of getting the job's ID, we have (what I assume is) the image as **Base64**. Saving it locally is optional (and disabled by default), so technically we don't even have to fill the server with a bunch of images which might be deleted anyways.

The thing is though, **we cannot "trust" the connection** to stay up the whole time, so these base64 images would still need to be stored somewhere in case the client disconnects suddenly, so at that point, we're just better off **storing the image**.

Agent Scheduler's approach to create an API that did basically the same stuff and was structured the same way as the original txt2img API was really good for reusability, which is why I am going to do the same thing. Instead of making an extension though, i am going to make a python api that does the following :
- Receive API requests
	- if the request can be fulfilled by one of its provided services, it executes it
	- if the request is not part of its provided services, it acts as a **middleman** between the client and stable diffusion.

by doing this, the user only needs to add a single endpoint. This helps in case we need some functionalities that are implemented by the Automatic1111 API but we don't  wanna rewrite them or have to manage multiple endpoints. 

this "super-api" (cuz it's a superset of the Automatic1111 API, by definition) also has the best excuse ever to introduce [Error 418 : I'm a Teapot](https://www.rfc-editor.org/rfc/rfc2324#section-2.3.2), since it incapsulates its meaning perfectly ([like in this Stackoverflow post](https://stackoverflow.com/questions/52340027/is-418-im-a-teapot-really-an-http-response-code)) as it's saying "yeah, I know what you're trying to do, but I'm not the right one to ask for it.", redirects your request to the stable diffusion client and returns "418" if the request could actually be fulfilled by stable diffusion, or the error response it provided otherwise.
