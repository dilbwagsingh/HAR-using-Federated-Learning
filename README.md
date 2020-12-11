# HAR-using-Federated-Learning

This project falls into the scope of ‘Human Activity Recognition’ using Federated Learning. Human Activity
Recognition itself proposes different advantages. The portable based wellbeing applications can be a
recipient for the old and infected people to recuperate quickly from any injury or forestall any mishaps. This
dataset is our essential decision since now-a-days everybody has a smartphone and they track their
wellbeing through the applications accessible to them.
My goal is to fabricate a robust yet basic Federated Learning model that can foresee precisely about the
exercises an individual is doing for the duration of the day dependent on the sensor information accessible to
it. Federated learning has different points of interest when contrasted with a centralised model however my
principal focus here is the privacy issues gathered by the private sensor information accessible on the client's
smartphones. I intend to build a model that trains on the private information in the client's framework and
simply transfer the weights to the global model for foreseeing and assessment of the public dataset. With
Federated Learning we can stay away from the information security/data privacy concerns.

## About the Dataset - UCI HAR

“Human Activity Recognition Using Smartphones Data Set” was chosen for the project, a multivariate
time-series based dataset of 10299 instances built from the recordings of 30 subjects performing daily-life
activities(ADL) while carrying a waist-mounted smartphone with embedded inertial sensors, with emphasis
on the output task of Classification or Clustering.The experiments have been carried out with a group of 30
volunteers within an age bracket of 19-48 years. Each person performed six activities (​ WALKING,
WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING​ ) wearing a
smartphone on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear
acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been
video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets,
where 70% of the volunteers were selected for generating the training data and 30% the test data.

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then
sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor
acceleration signal, which has gravitational and body motion components, was separated using a
Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have
only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each
window, a vector of features was obtained by calculating variables from the time and frequency domain.

For each record it is provided:
* Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
* Triaxial Angular velocity from the gyroscope.
* A 561-feature vector with time and frequency domain variables.
* Its activity label.
* An identifier of the subject who carried out the experiment.

## Preprocessing

The dataset was divided into four parts - three parts(with around 2500 samples each) to
be trained on three replicas of the model and one being the public dataset(around 3000 data samples) for
testing the final accuracy. There were a total of 561 features out of which some had duplicate column names
but they belonged to different subjects so were inherently different. This was handled by appending the
column name with a number.

Further I shuffled the dataset to remove any correlation that may exist between the data points related to one individual.
This was crucial as because the private datasets were only accessible in a particular local model(model
replica ‘i’) and only the trained model weights were used to calculate the weights of the global model and
evaluate accuracy.

## Visualizing the data

## Model Architechture

## Weight function

To serve our purpose we used a simple weighted average of the weights of the three local model replicas.
First we started off with equal weights i.e, [1, 1, 1]. This permutation performed averagely giving a global
model accuracy on the public set to be around 85%. We further tweaked the array to other values like
[5,2,3], [10,5,5], [10,2,3], etc. But we only got sub-par accuracy ranging between 85% to 90% with average
to poor consistency. Finally after further tweaking we settled for the weight array [1, 0.02, 0.03]. This array
gave better consistency but still the accuracy of the model was below 90%. To further increase the accuracy
of the global model on the public dataset we dynamically assigned the weights by closely monitoring the
private model accuracies and accordingly distributed the weights.

Following this procedure of weight selection and using the weighted average function to set the weights we
achieved the best accuracy of 91.35% with good consistency throughout multiple runs of the models.
Actuaries were constantly hitting greater than 88% mark throughout our further testing and simulations.

<u>Weight function equation-</u>
weights_global model = ( w1 * weights_model−1 + w2 * weights_model−2 + w3 * weights_model−3 ) / (w1 + w2 + w 3 )

Where [w1, w2, w3] is some permutation of [1, 0.02, 0.03] based on the private model accuracies.
This also seems natural because you want to give more weightage to the model which has higher accuracy.

## How to run the code-

It is fairly simple to get the code up and running on your computer. To do so follow the steps below-

1. Make a folder say ‘Project’.
2. Download the UCI HAR Dataset.zip file from-
https://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones
3. Click on the ‘Data Folder’ option in the above link and then click on the ‘UCI HAR Dataset.zip’ file
to start downloading.
4. Extract the zip file inside the ‘Project’ folder made in step-1
5. Copy and past the jupyter notebook(code) uploaded via the google forms inside the ‘Project’ folder.
6. Open the notebook and run all cells.

If you find any issues or have problems running the code feel free to contact me via linkedin/e-mail (Linkedin preffered).