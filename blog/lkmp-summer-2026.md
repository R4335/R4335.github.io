# My Linux Kernel Mentorship Experience

I have been learning and writing C since 2017 (almost a decade), mostly CLI tools for reversing and pwning, built for research. I am not really a UI or UX person. I like the terminal. I like code that does one thing well and talks to other code through pipes and files. So when I started looking for something to work on, the linux kernel was a natural fit. It's the largest piece of C code that I knew on the internet, because this linux kernel was almost used by any systems all around the world... by reading some blogs I can say 4 billion devices all around the world uses this linux kernel. It runs in the terminal you can't even see and it has no UI to design. This thing made me to apply **Linux Kernel Mentorship Program (LKMP) Summer 2026** cohort.

In simple, this mentorship is a kind of program that connects you with experienced kernel developers and walks you through your first real contributions to mainline linux 

## Staging: First Contact

I started in the staging tree like most new contributors. My first mentorship patch was [staging: iio: frequency: ad9832/ad9834:](https://lore.kernel.org/all/20260807094704.5371-1-raizudeen.kerneldev@gmail.com/T/#u). I noticed a helper function was being used oddly, but one of the maintainer pointed out that the fix was not as simple as I thought. I ended up turning it into documentation patch instead. A comment explaining how the helper worked, so the next reader wouldn't struggle like I did. Four versions later the maintainer reviewed the final one.

The thing i learnt from this patch was, when a reviewer pushes back on the what, sometimes the right answer is to change the what, not to defend the original.

## The Feel when I get real contributor

After the staging patch, I wanted real bugs. 
I found a 32-bit config write bug in the Intel Root Port MPC ACS quirk since 2014. I found that initially by understanding the logic but understanding logic itself is not good. So I tested that on my old laptop t420(thank god to have that) I found a logic issue there also and verified the issue that was real so I sent patch v1 and v2 on the same day. Then the PCI subsystem maintainer(Bjorn Helgaas) and fellow maintainer reviewed it and my first patch got accepted, few weeks later it landed on the mainline linux kernel. Then Greg backported it to several stable kernels from 5.10,5.15,6.1,6.6,6.12,6.18 and 7.2. That was the first time I felt like a real contributor.

## PCI Quirks and Crypto Internals

Over the mentorship I worked across two subsystems, crypto and PCI. I picked them because both sits at the boundary where software meets something harder than itself.<br> crypto is software wrestling with math and logic. PCI is software talking to actual hardware on a real motherboard. Both rewards for caring about the small things like bytes, alignment and the exact contract between the layers.  

Some patches that stood out:



