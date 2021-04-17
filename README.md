# WarGAN

## Creating a Generative Adversarial Network (GAN) to generate images of models in the Warhammer INQ28 style, based on images drawn from the community.
Project is being done as part of a article for 28Mag: https://28-mag.com/, which focuses on using creative engines as a form of art. As the engine is built on submitted minatures, it gives us a way to visualise the "collective memory" and intepretation of the "what is INQ28?"

This "collective memory", is made up of the features of the images, and the way in which features relate to each other, which are present across all of the submitted minatures. These relationships, form the basis for Generator creativity; they are the inspiration which the Generator uses to create new images. Much like how an artist remembers (their interpretation of) what a something might look like, based off their knowledge of all past examples they've seen, the Generator uses the identified features and relationships to inspire new art. 

### First dream 

![test](https://user-images.githubusercontent.com/80669114/114171485-73a92900-9988-11eb-9cbc-7b644b133ae5.jpg) 

## Plan
At this stage, I'm still playing around with model parameters at the moment, hunting for something that doesn't collapse, and which gives distinct images, that also fit the INQ28 style.

Current thoughts:
* It's looking like a Learning Rate of *0.00015* produces viable images, *lr=0.0002* tends to be dark blobs surrounded by gritty noise, while at *lr=0.0001* the drop off in improvement from more epochs doesn't allow enough time for the Generator to improve sufficently, resulting in white grainy images.
* Batch size seems to be the most significant variable for whether it creates distinct images or noisey blobs.
* Epochs greater than 50 seem to produce negligible improvements, but that might be an issue with how I'm loading the generator (shouldn't be, it's identical to how it was loaded in other projects). After many many generations (in this case *epochs > 300*), the Generator develops weird strategies and gets too good at beating the Discriminator, and is able to fool the Discriminator with it's images, even though they wouldn't pass a human test. The Generator learns to exploit a difference between real images and how the Discriminator internalises real images:

![dream2](https://user-images.githubusercontent.com/80669114/114269809-f7344a00-9a5c-11eb-90d0-edb21fb157ef.jpg)

### Visualisation of Model Training: 
Need to click on and open, doesn't loop

![traniningAnimation](https://user-images.githubusercontent.com/80669114/114508870-fa138280-9c88-11eb-939d-fcc239fb65fa.gif)



## Open access to the Generator file:
The final *.h5* model file used to generate images is too large to upload to Github. Will find a work around, hosting it somewhere else, so people can use the generator file to create local instances.

That, said, as the image distribution is fairly small, even with large batch sizes, the Discriminator converges a fairly stable representation of the whole dataset earlier than the Generator, so Generator doesn't get to try multiple strategies. As a result, the Generator file from a specific training session will create almost identical images, even with different random inputs (*"latent points"*)
