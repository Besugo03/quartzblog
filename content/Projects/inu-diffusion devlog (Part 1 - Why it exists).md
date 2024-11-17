##### Table of contents
- [[#Why jobs?|Why jobs?]]
- [[#How do you solve this then?|How do you solve this then?]]

If you have checked out my GitHub, it's possible you might have seen the [inu diffusion](https://github.com/Besugo03/inu-diffusion) project. It's basically a software that "latches into" stable diffusion, and adds its concept of "jobs" for queueing.

The project made huge leverage of the [Agent Scheduler Extension](https://github.com/ArtVentureX/sd-webui-agent-scheduler) for stable diffusion, borrowing its entire queueing system in order to generate the jobs.

From that point on, you can check and manage entire jobs (deleting them altogether, deciding which images to keep included in the job an which ones to delete, upscaling all the images in a given job etc...)

### Why jobs?
Stable diffusion's popular [Automatic1111 UI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) takes a single-image-centric approach (e.g. you CAN generate a batch of images, but you can only upscale or edit a single image at a time, you need to delete the images one by one if you don't like them...).

While this is relatively decent, once you get a little seasoned with it and you want to prototype a bunch of stuff (say, you want to try out different styles, or many different ideas for prompts for an image you want to produce) this gets really tedious.

Let's say I want to test out a new LoRa I made, and check out if my dataset was decent enough to not give it some weird biases when generating images.

1. I need to generate a bunch of images with wildly different prompts, settings, subjects etc...
2. once they are done, I need to check if the job actually went through correctly (I may have made a typo, referenced a wrong LoRa, put in the wrong resolution... usual user error)
3. if they are decent enough, I need to img2img every single image and their relative prompt to see how it treats higher resolution images
	- this is even more of a nightmare if, for each image I want to produce, I may want to try different settings like denoising strength, lora strength etc....

You can see how doing this stuff manually becomes a chore of its own. Not only that, but during that time my PC is basically screaming for help and I can't run anything significant on it since my GPU is held hostage by SD until it finishes.

The lower end your GPU is, the worse this gets, since it takes even more time out of your potential to actually use the PC.

Money can buy you a better GPU, but I need that money for more important stuff, like the new Factorio update.

![[Pasted image 20241117122605.png]]

### How do you solve this then?

With money out of the equation, the only other option we have left is time. Specifically, desynchronizing these generation operations.

Agent Scheduler's queueing system was halfway there. It provided API endpoints to put a generation in queue, interrogate its status, view the most recent queued generations and if they were done or not, their output images...

All that was left was to create some sort of local database of all the generations made up until now. And i don't know how databases work so i'm gonna create a big'ol JSON file.

The devlog continues here : [[inu-diffusion devlog (Part 2 - The data structure)]]