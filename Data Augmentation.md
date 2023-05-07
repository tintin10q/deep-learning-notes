# Data Augmentation

Data Augmentation is a technique to generate more data to train on. This is particularly effective during a specific classification problem. For instance finger print recognition. 

You can do:

- Translation / shifting
- Cropping
- Scaling
- Rotating
- Flipping
- Adding noise 
- ...

![Data Augmentation](Pasted%20image%2020220611155013.png)

You have to think about if this makes sense. For instance flipping does not make sense when you try to recognize written characters. 

I think the adding noise to images works really well because than your model becomes a lot more robust. 

Types of augmentation: 

## Audio
- Small shifts /cropping
- Frequency shift
- Reversing 
- Wrapring 
- Maksing regions

## Text
- Random subsequenes 
- Synoyms 
- Maksing words 
- Duplication words