After testing a lot of different things with inu-diffusion (mainly **wildcards** and **bangtags**, which we will probably talk about in a separate devlog, with the latter being already documented in the GitHub) i came across fairly few little issues regarding compatibility. So something was probably wrong.

I used Linux for the better part of the last year, and never looked back up until now, when I wanted to learn Premiere and started having a bit of nostalgia for Fusion360 for 3d modelling. Sadly there are some things that Linux still can't provide, so I reinstalled windows.

After fighting a frustrating battle against Windows' dominatrix-like attitude towards your privacy and what it knows best for you (like harvesting telemetry 24/7, re-activating the Windows Defender after I de-activated it multiple times just to remove MY OWN .exe of inu-diffusion because it was classified as some sort of DEFCON-1 threat) I finally managed to have a tolerable installation thanks to [Win11Debloat](https://github.com/Raphire/Win11Debloat) and [Windows defender remover](https://github.com/ionuttbara/windows-defender-remover).

![[Pasted image 20241117140038.png]]

---

Then disaster struck.

My forge stable diffusion installation was not launching anymore, probably some kind of encoding differences between Linux and windows. At that point I figured :

"well... at this point, might as well update this prehistoric installation, there might me lots of performance patches I could've missed up until now"

Starting from a blank slate then.

![[Pasted image 20241117135725.png]]

After noticing this shiny new interface I learned something new. Automatic1111 (and its forge counterpart) moved to the newer Gradio 4 for serving the UI with python.

This though did NOT mean that all the extensions maintainers did aswell. Do you remember the **Agent Scheduler** extension? You know, the one i based this whole project about?

It's been about a year since updates dropped. And it doesn't work anymore.

![[Pasted image 20241117140242.png]]

--- 

after pondering on it for a while, I realized that basing an entire project on an extension of the UI of another project was dumb to say the least.

Yeah, there are some forks of the original extension which... kinda work for the newer version of Gradio, but the support for wildcards is buggy at best and at this point, it's better i manage this stuff by myself.

![[Pasted image 20241117140457.png]]

okay maybe not the the last one. Or the last two. But you get it.

Follow the next devlog [[inu-diffusion devlog (Part 4 - planning the queueing system)|here.]]