---
library_name: peft
base_model: codellama/CodeLlama-7b-hf
---

# Model Card: CodeLlama-7B BashGen (QLoRA)

This model is a fine-tuned version of CodeLlama-7B designed for generating accurate, runnable Bash scripts from natural-language instructions. It’s optimized for developer workflows, CLI automation, and teaching users how shell commands work through optional explanations.

## Model Details

### Model Description

The model is trained using QLoRA adapters on a curated dataset of NL-to-Bash pairs, real-world shell scripts, and troubleshooting examples. The goal is a compact, reliable assistant that can turn plain English into clean Bash code while maintaining correctness and structure.  

- **Developed by:** Prerana  
- **Model type:** Instruction-tuned LLM for Bash generation  
- **Languages:** English  
- **License:** Same as the base model (CodeLlama License)  
- **Finetuned from:** `codellama/CodeLlama-7b-hf`

### Model Sources
- **Repository:** Coming soon  
- **Demo:** Local Streamlit interface (not publicly hosted)

## Uses

### Direct Use
Developers can use the model to:
- Generate Bash scripts from natural-language prompts  
- Automate repetitive CLI workflows  
- Learn how commands work through optional step-by-step explanations  
- Prototype automation tasks without writing shell code manually  

### Downstream Use
The model can be plugged into:
- CLI copilots  
- Internal dev-tools  
- Streamlit dashboards  
- Workflow automation assistants  
- Code editors/extensions

### Out-of-Scope Use
The model isn’t suitable for:
- Executing generated Bash directly without review  
- Security-sensitive environments requiring strict command validation  
- Complex multi-language pipelines beyond Bash  
- Tasks unrelated to shell scripting

## Bias, Risks, and Limitations

The model may:
- Generate commands that are correct syntactically but unsafe operationally  
- Mis-handle ambiguous instructions  
- Produce platform-specific commands (Linux-biased)  
- Not catch destructive operations like `rm -rf` unless explicitly prompted  

### Recommendations
Always review generated commands before execution, especially on production systems.

## Getting Started

```python
from transformers import AutoTokenizer, AutoModelForCausalLM
import torch

model_id = "your-model-id"

tokenizer = AutoTokenizer.from_pretrained(model_id)
model = AutoModelForCausalLM.from_pretrained(
    model_id,
    torch_dtype=torch.float16,
    device_map="auto"
)

prompt = "Create a script that compresses all log files older than 7 days."
inputs = tokenizer(prompt, return_tensors="pt").to(model.device)
outputs = model.generate(**inputs, max_new_tokens=200)
print(tokenizer.decode(outputs[0], skip_special_tokens=True))
