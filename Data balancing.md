# Data Balancing

Data Balancing is making the classes of your classification equal. You do htis because class imbalence can make it difficult to optimze a model. Because the model might just 

If you have a dataset with 80%/20% split than the model might just learn to always predict the class which ocures more ofthen and than 80% accuracy becomes might meaningless. 

## Class balancing stratagies 

- Use a weighted loss function
	- Give a lager weigth to smaples from the minority class
- Resample the dataset
	- Include more samples from the minorty class to create a balenced training set
- Keep it as is
	- Sometimes the imbalence reflects the real world and then the imbalence is a valid stratagy
- Make the class imbalence stronger
	- You could even make the imbalence stronger if this reflects the real world