---
layout: post
title: "How did I run SakuraLLM with my Intel Arc B580 on Windows"
author: the42game
categories: guides
tags: b580 llm
description: SakuraLLM(Qwen2.5) and generally llama.cpp on Intel Arc hardware HOWTO
---

# TL;DR and contexts as always:

[SakuraLLM][sakurallm-gh] is a LLM model trained specifically for translating Japanese ACGN contents (LNs and VNs) to Chinese, based on Alibaba's [Qwen][qwen-gh].

And are mostly use with the amazing easy to use tools provided by [auto-novel][auto-novel-hp] ([Github][auto-novel-gh]) to translate Japanese Light Novels (that are being serialize online) into Chinese, at least for me.

And for something with such a massive(and also niche) audiences, there must be some easy to use setup tools, which there are: Some rather powerful and easy to use tools like [Sakura Launcher GUI][sakura-launcher-gh] and [OneClickLLAMA][1clickllama-gh].

With the support centers around NVIDIA's CUDA, which is the dominant of GPU market as of the time of writing, with additional supports for APIs such as AMD/ATI's ROCm, Apple's Metal, and even if you have none of those, fallback to VULKAN for a wider range of support (Everything after Intel HD4000 got some kinds of Vulkan supports anyway...), but using Vulkan as the API might hinder some of the performance.

As a huge fan of Yuri-related contents in Japanese (Since 2014), that still can't be able to read too complex Japanese, I sure have a agenda to fill which is to run this LLM locally, without waiting others to run it for me.

With tight budget and constant look out on bargain pricing (and my anti-nvidia tradition) in mind, I went for an Intel Arc B580 GPU instead, the best GPU you could get for the price.

But, Santa Clara, we had a problem.

## Whats the problem?

Well as you can tell, Intel's Arc was pretty new at this point, so there aren't too many supports arounds it, and the financial struggles sure didn't help that.

So in the end, the easiest way for me to run it is via Vulkan, but I don't want to.

And there is [IPEX-LLM][ipex-llm-gh], an LLM acceleration library for Intel GPUs like Arc, which thanks to the Intel team behinds it, they provide intergration of llama.cpp and ollama.

For me, I am going to run [llama.cpp][ipex-llm-zip-qsg] on Windows (Shout out to the Intel guys for providing a Portable version thats saves a lot of time) but it will also works on Linux as well. And the end results was pretty nice but require some level of care taking in the process.

## HOWTO STARTS HERE:

Follow the guide till [here][ipex-llm-here]. Prepare yourself a .bat file in the same folder as your llama-cli/-serve.exe, don't forget to download the actual .gguf files and put them into the same folder or any place you want! For Arc B580 I suggest you to use [sakura-14b-qwen2.5-v1.0-iq4xs.gguf][sak-gguf-hf] for saving on VRAMs (which fits right into the 12GB VRAM of the B580).

Open your whatever bat file and put:

```batch
set ONEAPI_DEVICE_SELECTOR=level_zero:0
set ZES_ENABLE_SYSMAN=1
set SYCL_CACHE_PERSISTENT=1
set SYCL_PI_LEVEL_ZERO_USE_IMMEDIATE_COMMANDLISTS=1
llama-server.exe -m PATH/TO/SAKURALLM.GGUF -ngl 47 -fa -c 6194 -v -t 20 -a sakura-14b-qwen2.5-v1.0-iq4xs.gguf --host 0.0.0.0 --port 11434
```
Change the PATH/TO/SAKURALLM.GGUF to your file path, save the file, and run this .bat file and you are DONE(in an good way). The only thing you would needs to do is to open [auto-novel][auto-novel-hp] and do your thing.
if it complains about thread or sth like that set `-t` to something like `8`

Lght, let me expalins all the necessary components of this bat file and what they do:

`set ONEAPI_DEVICE_SELECTOR=level_zero:0`: If you have multiple Intel GPUs(including the iGPU inside your CPU), you should manually select which one to use, or you will face severe performance punishment due to the bandwidth between CPU and GPU ain't all that great... Please refer to this part of the guide.

The recommended settings from devs in the README file in the zip, seems to enhance performance:

```batch
set ZES_ENABLE_SYSMAN=1
set SYCL_CACHE_PERSISTENT=1
set SYCL_PI_LEVEL_ZERO_USE_IMMEDIATE_COMMANDLISTS=1
```

And lastly:

```batch
llama-server.exe -m PATH/TO/SAKURALLM.GGUF -ngl 47 -fa -c 6194 -v -t 20 -a sakura-14b-qwen2.5-v1.0-iq4xs.gguf --host 0.0.0.0 --port 11434
```

Runs the actual LLAMA.CPP server itself, with `-ngl` set to `47`, `-c`(context) set to 6194, and had `-t` set to 20 (I am using a Intel i5-14600K here.), and let LLAMA-SERVER to tell REST API which model its using by `-a sakura-14b-qwen2.5-v1.0-iq4xs.gguf` change it accordingly to the actual model name you use

and lastly are the address and ports to run the server, change it to you liking.

## Some performance figures.

I've gather something around 10 to 12 tokens/s with i5-14600K plus 2*32GB DDR4 RAM and Intel Arc B580 itself.

And please remember never let it running out of VRAM, I'VE SUFFERED FRIN SEVERE PERFORMANCE PENALTY FOR THAT AND I SPENT A WHOLE NIGHT TO FIGURE OUT THAT.

At last, huge thanks to Intel Arc's team, you guys are amazing for providing something like this. And keep up the good work Intel, we really need someone to compete with Nvidia and AMD(albeit AMD barely did anything tbh... I am really disappointed).

And ALSO HUGE THANKS TO THE GUYS BEHIND SAKURALLM AND DEVS OF THE TOOLS AND WEBSITES, without them i might still be hard reading those lovely tasty yuri light novels in Japanese in pain...


P.S: I might re-write this whole things in Chinese later on for helping others, but lets face the facts, the one who will need this guide are either: the one who had a laptop with Intel Arc Graphics and tons of RAM in it, or geeks who already know how to do. So I hope I would get a Chinese version done if I had the time lol, for the sake of the former.

If you have any problem or suggestion to this article, please feel free to reach out to me on Telegram via [here][tg-ch-link], you know the drill, I am the only person in the Channel who could send posts.


Thank you for reading.



[sakurallm-gh]: https://github.com/SakuraLLM/SakuraLLM
[qwen-gh]: https://github.com/QwenLM/Qwen
[auto-novel-hp]: https://n.novelia.cc
[auto-novel-gh]: https://github.com/auto-novel/auto-novel
[sakura-launcher-gh]: https://github.com/PiDanShouRouZhouXD/Sakura_Launcher_GUI
[1clickllama-gh]: https://github.com/neavo/OneClickLLAMA
[ipex-llm-gh]: https://github.com/intel/ipex-llm
[ipex-llm-zip-qsg]: https://github.com/intel/ipex-llm/blob/main/docs/mddocs/Quickstart/llamacpp_portable_zip_gpu_quickstart.md
[ipex-llm-here]: https://github.com/intel/ipex-llm/blob/main/docs/mddocs/Quickstart/llamacpp_portable_zip_gpu_quickstart.md
[sak-gguf-hf]: https://huggingface.co/SakuraLLM/Sakura-14B-Qwen2.5-v1.0-GGUF/blob/main/sakura-14b-qwen2.5-v1.0-iq4xs.gguf
[ipex-llm-gpu-sel]: https://github.com/intel/ipex-llm/blob/main/docs/mddocs/Quickstart/llamacpp_portable_zip_gpu_quickstart.md#error-detected-different-sycl-devices
[tg-ch-link]: https://front.the42.info/ch