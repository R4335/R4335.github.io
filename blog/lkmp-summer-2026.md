# My Linux Kernel Mentorship Experience

![LKMP MENTORSHIP SUMMER 2026](https://raw.githubusercontent.com/R4335/R4335.github.io/main/blog/assets/lkmp_summer_2026.webp)

I have been learning and writing C since 2017 (almost a decade), mostly CLI tools for reversing and pwning, built for research. I am not really a UI or UX person. I like the terminal. I like code that does one thing well and talks to other code through pipes and files. So when I started looking for something to work on, the linux kernel was a natural fit. It's the largest piece of C code that I knew on the internet, because this linux kernel was almost used by many systems all around the world... by reading some blogs I can say 4 billion devices all around the world uses this linux kernel. It runs in the terminal you can't even see and it has no UI to design. This thing made me to apply **Linux Kernel Mentorship Program (LKMP) Summer 2026** cohort.

In simple, this mentorship is a kind of program that connects you with experienced kernel developers and walks you through your first real contributions to mainline linux 

## Staging: First Contact

I started in the staging tree like most new contributors. My first mentorship patch was [staging: iio: frequency: ad9832/ad9834:](https://lore.kernel.org/all/20260807094704.5371-1-raizudeen.kerneldev@gmail.com/T/#u). I noticed a helper function was being used oddly, but one of the maintainer pointed out that the fix was not as simple as I thought. I ended up turning it into documentation patch instead. A comment explaining how the helper worked, so the next reader wouldn't struggle like I did. Four versions later the maintainer reviewed the final one.

The thing i learnt from this patch was, when a reviewer pushes back on the what, sometimes the right answer is to change the what, not to defend the original.

## The Feel when I get real contributor

After the staging patch, I wanted real bugs. 
I found a 32-bit config write bug in the Intel Root Port MPC ACS quirk since 2014. I found that initially by understanding the logic but understanding logic itself is not good. So I tested that on my old laptop t420(thank god to have that) I found a logic issue there also and verified the issue that was real so I sent patch v1 and v2 on the same day. Then the PCI subsystem maintainer(Bjorn Helgaas) and fellow maintainer reviewed it and my first patch got accepted, few weeks later it landed on the mainline linux kernel. Then Greg backported it to several stable kernels from 5.10,5.15,6.1,6.6,6.12,6.18 and 7.2. That was the first time I felt like a real contributor.

## PCI Quirks and Crypto Internals

Over the mentorship I worked across two subsystems, crypto and PCI. I picked them because both sits at the boundary where software meets something harder than itself.<br> crypto is software wrestling with math and logic. PCI is software talking to actual hardware on a real motherboard. Both rewards for caring about the small things like bytes, alignment and the exact contract between the layers.  

Nine patches accepted, each with a lesson:

- **[[PATCH v2] PCI: Fix 32-bit config write in Intel RP MPC ACS quirk](https://lore.kernel.org/all/20260723171203.4892-1-raizudeen.kerneldev@gmail.com/T/#u) :** First accepted patch, backported to 7 stable trees. Tested on my old T420. Lesson learnt was testing on real hardware before sending matters.
- **[[PATCH v3] PCI: quirks: Fix out-of-bounds MMIO read in nvme_disable_and_flr()](https://lore.kernel.org/all/20260903164615.5744-1-raizudeen.kerneldev@gmail.com/T/#u) :** Read past the end of a mapped memory region. After five weeks of silence following Alex's Reviewed-By, a polite ping got it applied to pci/misc for v7.4. The lesson learn was maintainers aren't annoyed by pings, just they are busy.
- **[[PATCH v4] staging: iio: frequency: ad9832/ad9834: add comment explaining do_div usage](https://lore.kernel.org/all/20260807094704.5371-1-raizudeen.kerneldev@gmail.com/T/#u) :** First mentorship patch, originally a code fix that became a documentation patch after Andy's review. Four versions.
- **[[PATCH v2] crypto: amlogic: Fix DMA memory leak in cipher error path](https://lore.kernel.org/all/20260822084509.26385-1-raizudeen.kerneldev@gmail.com/T/#u) :** A cipher operation could leak DMA memory if it failed mid-flight. From this patch I learnt that error paths are where the real bugs hide.
- **[[PATCH v3] crypto: amlogic: Fix IRQ handler return value and fallthrough logic](https://lore.kernel.org/all/20260918115950.9121-1-raizudeen.kerneldev@gmail.com/T/#u) :** A missing return led the handler fall through into the wrong case.
- **[[PATCH v4] crypto/amlogic: Use devm APIs for clock management](https://lore.kernel.org/all/20260916174227.5581-1-raizudeen.kerneldev@gmail.com/T/#u) :** In this patch I replaced manual `clk_prepare_enable()` / `clk_disable_unprepare()` with `devm_clk_get_enabled()`. I couldn't forget this one :) because this is the patch that later broke someone else's submission. Every patch landed on the linux kernel reshapes the surface area future contributors work on.
- **[[PATCH v2] crypto: x86/aes-gcm - fix always true check for last AAD segment](https://lore.kernel.org/all/20260926145903.6061-1-raizudeen.kerneldev@gmail.com/T/#u) :** An efficiency bug doing extra `memcpy()` work per segment, not breaking correctness, so the crypto self-tests never caught it. Backported to 6.18 and 7.2 stable trees.
- **[[PATCH v3] lib/crypto: chacha20poly1305: zeroize state in __chacha20poly1305_decrypt](https://lore.kernel.org/all/20261005150324.5621-1-raizudeen.kerneldev@gmail.com/T/#u) :** I closed a small information leak window where intermidiate state lingered on the stack. It was reviewed by three senior maintainers (Eric Biggers, Ard Biesheuvel and Jason Donenfeld). The thing I learnt from this patch was security relevant code gets more eyes and that's a feature.
- **[[PATCH v3] lib/crypto: md5: remove unused header](https://lore.kernel.org/all/20261016023558.8797-1-raizudeen.kerneldev@gmail.com/T/#u) :** I removed unused header `<linux/unaligned.h>` include. Herbert Xu caught an unnecessary `bitops.h` addition in v1. Then Eric Biggers caught an inaccurate commit message in v2. v3 finally matched the reality. One line removed, three versions. The lesson I learnt was, commit messages become part of the historical record, so they have to be accurate.

These are accepted patches only, I have patches those were not accepted and not reviewed yet and for some patches I had really great discussion with the reviewers and good feedback from maintainers that led me to withdraw my patches after knowing the real issue where it triggers.


## Something Strange

Later in the mentorship, I got CC'd on a rejection of someone else's patch. The fellow contributor had send a carefully reasoned amlogic IRQ cleanup, but the code they were trying to fix no longer existed. Because my own earlier `devm_clk_get_enabled()` conversion hadremoved it and Herbert Xu pointed them at my commit.

Every patch that lands in the kernel changes what the next person sees when they open the file. The code we change today will be read and worked on by other contributors tomorrow. That changed how I think about even small patches.

## The Beginning :)

If there's one thing that this mentorship taught me, its that **patience is everything** in the Linux kernel. Patches sit for weeks, reviews come back with feedback, versions may get higher. The contributors who last are the ones who learn to wait without taking it personally annd who keep on working the next patch while the previous one sits in the queue.

One thing I really want to mention, the [Linux kernel documentation](https://docs.kernel.org/) is far better than its reputation. [Submitting patches](https://docs.kernel.org/process/submitting-patches.html) was my constant companion. The [crypto API docs](https://docs.kernel.org/crypto/index.html) explained how the subsystem was organized. Whenever I was not sure about something, the answer was usually already written somewhere under [docs](https://docs.kernel.org/). The kernel is one of the few codebases where reading the docs first will save you hours of guessing. In simple Kernel docs is like a cookbook.

The community was welcoming in a way I didn't expect, every maintainer, reviewer and fellow contributors who reviewed my work treated it seriously, nobody was too junior to get a real review and nobody was too senior to be kind. I plan to keep contributing after the mentorship ends. Thanks to Shuah Khan, Brigham Campbell and Jori Koolstra for mentoring. Special thanks to the Linux Foundation for this opportunity.
