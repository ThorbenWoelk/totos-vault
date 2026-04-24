---
created: 2026-01-09
last_edited: 2026-04-24
tags:
- comfyui
- local-image-gen
- checkpoints
- split-models
- lora
- k-sampling
- fp8
- gguf
- quantization
- macos
connections:
- '[[001 Knowledge Base/AI/_index|AI Index]]'
- '[[2025-12 AI Recap]]'
- '[[2026-01-16 Small Is Beautiful]]'
ai_generated: false
human_approved: false
category:
- Knowledge Base
- AI
- Local ImageGen
---
###  The Crash Course: Model Types

ComfyUI is an image gen app. It allows you to load models in pieces or as a whole package.

- **Checkpoints (`.safetensors` in `models/checkpoints`)**:  
    This contains everything needed to generate an image (UNet + CLIP + VAE).
    - _Examples:_ SD1.5, SDXL, Juggernaut XL, Pony Diffusion.
    - _How to use:_ Use a **"Load Checkpoint"** node in ComfyUI. It creates the model, clip, and vae connections for you.
- **Split Models (UNet / CLIP / VAE)**:  
    - **UNet:** The diffusion model that draws the image (`models/unet` or `models/diffusion_models`).
    - **CLIP:** The text encoder that read your prompt (`models/clip` or `models/text_encoders`).
    - **VAE:** Turns the code into pixels (`models/vae`) - image or video. 
    - _Examples:_ **Flux.1**, **Ovis** (your current one), SD3.
    - _How to use:_ You use three separate loader nodes (`UNETLoader`, `CLIPLoader`, `VAELoader`). 
- **LoRA (`.safetensors` in `models/loras`)**:  
    Creates a specific style, face, or object.
    - **IMPORTANT:** A LoRA is **NOT** a standalone model. You cannot "replace" Ovis with a LoRA. You must _add_ a LoRA to a compatible base model.
    - _How to use:_ Insert a **"Load LoRA (Model Only)"** node between your Model loader and the KSampler.

### The Crash Course: KSampling

#### 1. Steps (The "Effort")

How many times the AI stops to refine the image.

- **Low (1-10):** The AI rushes. Results look blurry, unfinished, or like a rough sketch.
- **Standard (20-30):** The sweet spot. The image looks finished and detailed.
- **High (50+):** The AI overthinks. It might add too much detail (weird textures) and it wastes your time.
    - _Note:_ "Lightning" or "Turbo" models only need 4-8 steps. Normal models need 20+.

#### 2. Sampler Name (The "Artist's Style")

The strategy the AI uses to draw.

- **`euler_ancestral` (euler_a):** The "Creative Chaos" artist. It adds a bit of randomness every step. Results are diverse and creative, but the image structure changes if you add more steps.
- **`dpmpp_2m` / `dpmpp_sde`**: The "Precision" artist. Very sharp, realistic, and stable. Great for photorealism.
- **`lcm`**: The "Speed Demon". Specifically for Lightning/Turbo models. Don't use this on normal models.

#### 3. Scheduler (The "Pacing")

How the AI manages its "noise removal" budget over the steps.

- **`normal`**: Standard pacing.
- **`karras`**: The Fan Favorite. It spends more time on the fine details at the end. Makes images look sharper and cleaner.
    - _Pro Combo:_ `dpmpp_2m` (Sampler) + `karras` (Scheduler) is the "Gold Standard" for realism.
- **`sgm_uniform`**: Often used for Video or special models (like Flux).
- **`simple`**: Often used for Flux.

#### 4. Denoise (The "Creativity License")

Crucial for **Image-to-Image** (when you start with an existing picture).

- **1.00 (100%):** "Ignore the original image completely. Draw strictly what the prompt says." (This is default for Text-to-Image).
- **0.50 (50%):** "Keep the shapes of the original image, but change the colors/details to match my prompt."
- **0.10 (10%):** "Barely touch the image. Maybe just smooth it out a tiny bit."

#### Summary Cheat Sheet

- **For Realism:** Steps: 30, Sampler: `dpmpp_2m`, Scheduler: `karras`.
- **For Creativity:** Steps: 25, Sampler: `euler_ancestral`, Scheduler: `normal`.
- **For Flux:** Steps: 20, Sampler: `euler`, Scheduler: `simple`.

## On Mac

### My M4
- **Hardware Profile:**
    - **Safe Zone:** FP16, BF16, GGUF (Q4/Q8).
    - **Danger Zone:** FP8 (Native), massive unquantized models (>16GB).
    - **Successful Test:** `z_image_turbo_bf16` + `qwen_3_4b`.

### FP8
"FP8" (8-bit floating point) is a compression trick mainly optimized for NVIDIA's newest cards (4090s, H100s). Most image gen models are using it. Mac's GPU driver (Metal/MPS) does **not** natively support running these FP8 math operations directly. 1. **The Result:** When ComfyUI loads this `17GB` FP8 file on a Mac, it usually thinks, "I can't run this math," and **converts it back to FP16** (16-bit) in order to run it. **17 GB (FP8)** --> **34 GB (FP16)** in RAM. Out of memory error. Stick to **GGUF** versions if you want to run huge models like this on a Mac. GGUF is a format specifically designed to keep models compressed _while_ they run, preventing that RAM explosion.

