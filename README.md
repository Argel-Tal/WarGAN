# WarGAN
## Creating a Generative Adversarial Network (GAN) to generate images of models in the Warhammer INQ28 style, based on images drawn from the community.
Project is being done as part of a article for 28Mag: https://28-mag.com/, which focuses on using creative engines as a form of art. As the engine is built on submitted minatures, it gives us a way to visualise the "collective memory" and intepretation of the "what is INQ28?"

This "collective memory", is made up of the features of the images, and the way in which features relate to each other, which are present across all of the submitted minatures. These relationships, form the basis for Generator creativity; they are the inspiration which the Generator uses to create new images. Much like how an artist remembers (their interpretation of) what a something might look like, based off their knowledge of all past examples they've seen, the Generator uses the identified features and relationships to inspire new art. 

### First dream 

![test](https://user-images.githubusercontent.com/80669114/114171485-73a92900-9988-11eb-9cbc-7b644b133ae5.jpg) 


## Plan
* At this point I've found model parameters which produce interesting outputs. Model doesn't collapse, which is always nice. Seems to be able to pull itself out of weird pits, so no more mode collapse!! 
🎆🎆 
* Model outputs have distinct silhuettes, and have decent levels of internal texture/detailing, which fit the INQ28 style.
* From here I just want to try adding more detail/resolution to the images.

## Visualisation of Model Training: 
*You might need to click on and open it, as the GIF doesn't loop and might have played through while the page was buffering*

![traniningAnimation](https://user-images.githubusercontent.com/80669114/114508870-fa138280-9c88-11eb-939d-fcc239fb65fa.gif)


## Algorithm
The skeleton code of this project is based on code taken from: https://machinelearningmastery.com/how-to-develop-a-conditional-generative-adversarial-network-from-scratch/

### Generator code:
This is the architecture for the agent twin responsible for learning how to create new original images, the creative twin. From random noise, this agent is able to create images which fit the aesethic of INQ28, by implementing rules it's developed and learned autonomously.
![gener](https://user-images.githubusercontent.com/80669114/119077451-659afd80-ba48-11eb-8804-2c952713cfb3.png)

### Discriminator code: 
This is the architecture for the agent twin responsible for determining if an image it's presented with is a fake, or if it' real. This agent is key to development, as it's determination is what forces the Generator to improve and develop. Theoretically, it could also be used to determine if an external image fits into the aesethic of INQ28. As such, it is this twin who is determining the "underlying truth" of INQ28.
![discrim](https://user-images.githubusercontent.com/80669114/119077834-28833b00-ba49-11eb-8ddd-052e8f696218.png)

### Training sequence:
This function is what gets called when the twin agents should be developed. It inherets various hyper-parameters that are either explicitly decleared earlier, or implicitly used, as they'r defined for the Generator and Discriminator agents. 
Essentially, it subsets all the images in the training set into *batches*, upon which agents are trained. For every run of the loop, a new batch of images/data is sampled, preventing the system from overfitting one specific sample. 

Every 10 generations (i) the model is saved, and an image is generated and saved to a local storage location, specified in the function call. The sequence of images is then able to be collected and turned into a timelapse GIF/video. I use https://ezgif.com/maker
![trainGenLoop](https://user-images.githubusercontent.com/80669114/119077895-4486dc80-ba49-11eb-8d22-df965eace376.png)



## Current thoughts:
* It's looking like a Learning Rate of *0.00015* produces viable images, *lr=0.0002* tends to be dark blobs surrounded by gritty noise, while at *lr=0.0001* the drop off in improvement from more epochs doesn't allow enough time for the Generator to improve sufficently, resulting in white grainy images.
* Batch size seems to be the most significant variable for whether it creates distinct images or noisey blobs.
* Epochs greater than 50 seem to produce negligible improvements. After many many generations (in this case *epochs > 300*), the Generator develops weird strategies and gets too good at beating the Discriminator, and is able to fool the Discriminator with it's images, even though they wouldn't pass a human test. The Generator learns to exploit a difference between real images and how the Discriminator internalises real images, a symptom caled *"mode collapse"*, resulting in weird outputs like this: 

![dream2](https://user-images.githubusercontent.com/80669114/114269809-f7344a00-9a5c-11eb-90d0-edb21fb157ef.jpg)

* Current objective is to add more detail to the images. I'm looking into super resolution GANs which are mostly used in image restoration and de-blurring of existing images. Haven't assessed the additional gains of what little I've implemented for this yet.



## Open access to the Generator file:
The final *.h5* model file used to generate images is too large to upload to Github. I will find a work around, hosting it somewhere else, so people can use the generator file to create local instances, likely Dropbox or GoogleDrive.

That said, as the image dataset is fairly small, even with large batch sizes the Generator converges a fairly stable representation of the whole dataset. As a result, the Generator file each training session will create almost identical images, even with different random inputs (*"latent points"*), as the dataset is not wide enough to force the agents to develop multiple strategies. Because of this, having a site like https://thispersondoesnotexist.com doesn't make much sense, unless it's hosting a dataset of pre-generated images, rather than generating new ones when visited 😞
