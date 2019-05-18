# Hackintosh-Virtulization-FAQ

So I felt I needed to make this post after I noticed quite a few posts on this subreddit asking fairly basic questions over and over again so I feel we should try and clear up any questions and concerns people have about running MacOs in a VM over running bare metal.

# So what’s the difference between a bare metal Hackintosh and running a hypervisor?

So running bare metal means you’re running MacOs on the hardware itself without any layers in between, this is how most people run their operating systems like Windows and Linux as well and this will provide the most performance. A virtual machine by contrast must run on something else, in this case it would be a hypervisor. A hypervisor is used as a light weight control centre for managing all your VMs, common hypervisors include:

* [ESXI](https://www.vmware.com/ca/products/esxi-and-esx.html) (Paid and free licence available)
* [Proxmox](https://www.proxmox.com/en/) (free)
* [Hyper V](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=2ahUKEwjZvP26yufhAhVHu54KHRCSCmEQFjABegQIBxAB&url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fvirtualization%2Fhyper-v-on-windows%2Fabout%2F&usg=AOvVaw2pETdaigCviD-8cQeh1L75) (included with Windows 10 Pro)
* [unRAID](https://unraid.net) (Paid but free trial)
* Linux distros with KVM support(Kernel Virtual Machine, free)

# Pros and Cons to Virtualization

So you may be wondering why go the virtualization route instead of just running MacOs the old hackontsh way? Well there's actually quite a few reasons to do this:

**Pros:**

* **Easy snapshots:** An OS update bricked your system? Just revert back to the last hour's sham-shot in seconds and be right back to work
* Allows for **hardware emulation** like for unsupported NICs, storage drives, audio controllers, etc. This is currently the only workaround to running 970 EVO Plus in MacOs and this helps guarantee support for products that may not have official support like 10GbE cards
* **Less kexts** like FakeSMC are not needed since they're emulated
* Having the knowledge to do this so when the inevitable ARM Macs come you'll know how to properly emulate the hardware(not that great of point as most people just watch youtube tutorials or use prebuilt tools so they learn nothing anyways)

**Cons:**

* **You need a GPU for both your hypervisor and MacOs**: This isn't really much of an issue for intel's consumer CPU users as they have access to their iGPU but unfortunate AMD users need to grab a spare GPU just for the hypervisor. You can pass the iGPU through to MacOs in certain hypervisors like ESXI and Proxmox but this doesn't always work and is considered more work than it's worth. Edit: As u/swgbex mentioned, you can run hypervisors in headless mode so you don't need to dedicate a GPU to the hypervisor full time
* **CPU/RAM overhead:** Since you have to both manage 2 operation systems in a sense, you loose about 7-10% in single core performance. This can be lessened a bit if you dedicate a core to the hypervisor but this isn't always ideal when running a 4 core, 8 thread CPU. With Geekbench, I saw 17% drop in singlecore and a 5% drop in multicore on my i7 6700k. Cinebench told a similar story as I saw a 13% drop
* **More to manage:** Now you have to deal with issues not just from MacOS playing nicely with your hardware but also managing your hypervisor, making sure everything up-to-date and small changes that can break your MacOS VM
* **Less online support:** even with the recent boom of virtualization, there's not really many places to go to to get help with MacOs. Even on r/hackintosh, we can't really help you with VM issues due to having less users running hypervisors. At least if an update breaks your system on a real Hackintosh, there's a far greater chance of someone knowing how to help you whereas the community around virtualization is a to smaller
* **GPU compatibility**: Think you got away from this one? Well you can actually get worse GPU sport when running a hypervisor, specially that Vega cards have issues properly being passthrough which plagues windows and linux users as well. Oh and you still need to [follow the recommend GPU list for Mojave](https://www.reddit.com/r/hackintosh/comments/b91vf5/mojave_gpu_buyers_guide/) so no Maxwell, Pascal or Turing support
* **Motherboard compatibility**: Surprisingly this can actually be worse with virtualization as certain controllers will refuse to be pass through, can't break up certain IOMMU groups, certain NICs not being compatible with the hypervisor(Realtek and ESXI don't play nicely, learned that the hard way)

# Should I virtualize over Hackintosh?

For most people, no. Hackintoshing is a far safer option for the reason that there's a much larger community to ask for help. Virtualization is fun for those who want to experiment but hackintoshing with the [Vanilla guide](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/) will get you a near perfect system

# Other Frequently asked questions

**Can I run MacOs on a VM and later boot it as a stand alone OS?**

* Yes, if you passed through a drive when making your VM then the drive is fully functional as a Hackintosh drive as long as the required kexts like FakeSMC are present and you have some form of emulated EFI(most common is Clover)

**What GPU's are compatible?**

* Same as that for hackintoshes and real Macs, check out the [Mojave's GPU Buyers Guide](https://www.reddit.com/r/hackintosh/comments/b91vf5/mojave_gpu_buyers_guide/) for more info

**What CPU's are compatible?**

* Anything with Virtualization support is supported, though it's recommended to have the newer AES instruction set

**What motherboards are compatible?**

* This is a hit or miss with most boards, you'll need to search up how well your motherboard supports breaking of IOMMU groups and how they handle different controller passthroughs. Some boards won't allow for NVMe passthrough while other may not allow for any onboard USB controller to be passed through

**What happens if I run a Maxwell, Pascal or Turing GPU in Mojave?**

* Zero hardware acceleration, MacOs becomes basically unusable

**What guide should I follow?**

* Unfortunately there is no good guide you can follow for all hypervisors. Some guides are better than other's but I'll link the ones I like. Remember google is your friend:
* [ESXI](https://www.insanelymac.com/forum/topic/337101-the-almost-definitive-guide-for-installing-macos-1014-mojave-on-esxi-67-wip/)
* [Proxmox](https://www.nicksherlock.com/2018/06/installing-macos-mojave-on-proxmox/)
* Hyper V(unfortunately there is no guide, it's more fix as you go)
* [unRAID](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=2ahUKEwjC07PZz-fhAhVUvJ4KHVhFCY4QwqsBMAB6BAgIEAQ&url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DYWT4oOz2VK8&usg=AOvVaw0sqm5Mil0n70adlqxzu3NR)
* [Linux with KVM](https://www.youtube.com/watch?v=ATnpEOo3GJA)(yeah we like to hate on LTT but the linux portion of the guide was fairly well done)

**Can I run my Nvidia GPU on an older version of MacOs to get acceleration?**

* Yes you can run High Sierra, though this only applies to Maxwell and Pascal. Turing GPUs have no support in any version of MacOs whatsoever

**Can I have multiple VMs on one drive?**

* Yes, in a sense each virtual machine is an image so you can compress, move them around and load them onto another system without much issue

**Can I Hackintosh and then run it in a VM?**

* Yes, though you'll want to remove some kexts that aren't needed like FakeSMC

&#x200B;

And if there's any other questions or critiques to add, I'd love to hear them!

\- Local neighbourhood Hackintosh Slav

&#x200B;

Edit 1: Grammar(credit to u/ChappyBirthday)

Edit 2: Thank you to u/zakkol for pointing out the ESXI trial vs licence and also the Hyper V part

Edit 3: Thank you to u/swgbex for correcting me on running hypervisors without a GPU
