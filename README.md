# aws-cft-llama-cpp


The idea behind this project was to develop a Cloudformation template that would perform all the actions
required to setup llama.cpp to run a large language model. It is intended for hobbyists and is optimized for low cost.
The Cloudformation template sets up a IAM role with a minimal set of permissions to enable AWS Session Manager Connect 
to make it easy to connect to the instance from the AWS web console.

Note : The UserData script in this template downloads a aproximately 4.5 GB model. It is meant to be done once. If you 
find yourself running this template a lot, please move the models to S3 or create a AMI with the model already on it.

### Requirements
- A standard AWS VPC with at least one public subnet with Public IP addresses enabled

### Approximate AWS Costs as of 2024. Subject to change
- c6a.xlarge : $113 / month (assuming you accidentally left it on all month)
- p3.2xlarge : $2234 / month (!!) (assuming you accidentally left it on all month)
- storage : $0.08 gigabyte/month

### Templates
- cft-llama-cpp.yaml - (Recommended) this is the original template. It uses CPU and is cheaper but slower
- cft-llama-cpp-gpu.yaml - This uses a NVIDIA GPU and is more expensive. It is theoretically faster

NOTE - Both versions are not fully optimized. THere are a lot of possible variables that can be set when 
running the server. The GPU version in particular could be optimized to offload more layers, and a larger model 
could also be used.

### Instructions
- Run the cloudformation template
- When it completes navigate to the url on the cloudformation outputs tab
- You should see a website with options. Leave them default for now. Enter a question in the text field in the bottom and hit 'send'
- Remember that as setup this is going over HTTP - so not encryped. Do not send anything sensitive
- Remember to turn the instance off when you are done (if it is off you will only be charged for storage)

### How to uninstall this project
- Selecting the Cloudformation stack and choose Delete
- Verify that the delete is complete. In rare circumstances you may have to do this twice if Cloudformation gets stuck

### How to restart the web server if you turned the instance off
- Turn the ec2 instance on from the amazon console
- Hit 'connect' to connect with system manager session manager
- sudo su
- cd /opt/llama.cpp
- ./server -m models/${ModelFileName} --host 0.0.0.0 --port 80

### Some Notes on Models / Performance / Etc
- You can download as many models as you would like to the models folder. The Linux command 'wget' works well for this.
- llama.cpp runs models in the gguv format. The default instance will run 7b models fairly well in this format.
- Generally RAM requirements roughly match the billions of parameters in a model (give a 7b like 8GB of RAM or so)
- Some of the 13b models are great - but CPU only performance might be not great.
- llama.cpp does support offloading work to GPUs. See the repo for more details 

### References
- [Llama.cpp](https://github.com/ggerganov/llama.cpp)
- [Hugging Face](https://huggingface.co/)


### License / Whatever
- Use at your own cost and risk
- Feel free to copy and have fun


