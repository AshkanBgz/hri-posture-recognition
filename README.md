# hri-posture-recognition

This is my part of the group project (The Unfiltered - Social cue analysis for Human-Robot Interaction).
While one of my teammates is working on facial expression recognition, I'm handling the body posture
side of things - basically trying to figure out from someone's posture (arms, shoulders, how they're
leaning, head position) whether they seem confident/engaged or closed off/disengaged. The idea is that
these two signals (face + body) get combined later to give the robot a better read on how the person
is actually feeling, since someone can smile and still have closed body language.

I originally planned to record our own video clips of people in different postures and label them
myself, but that fell through - my webcam is broken and getting the team together to record clips
was harder than expected. So instead I'm using a public dataset from Kaggle that already has these
posture features worked out (Confidence Detection Dataset - link below).

## What's in here
- `pose_module.ipynb` - the actual notebook, meant to run in Google Colab. Downloads the dataset,
  trains a classifier on it, and evaluates how well it does.
- `results/` - the outputs from running it: confusion matrix, feature importance plot, and the saved
  model file.

## Dataset
[Confidence Detection Dataset](https://www.kaggle.com/datasets/muhammadkhubaibahmad/confidence-detection-dataset)
on Kaggle. ~5950 rows of pose-landmark-derived features (things like wrist-to-shoulder ratio, shoulder
slope, spine angle) labeled as Confident / Neutral / Low. Only covers upper body stuff (head, shoulders,
arms) which is fine since that's basically all you can see from a webcam anyway.

## How I ran it
1. Open the notebook in Colab
2. You need a Kaggle API token for this - get one from your Kaggle account settings, upload it when
   the notebook asks
3. Just run all the cells in order

## Results so far
Trained a RandomForest on the features (using cross-validation, not just one train/test split so the
number is more trustworthy). Got **95.5% accuracy** on the held-out test set. The model mixes up
Neutral with Confident sometimes but Low confidence gets classified almost perfectly. Makes sense
when you look at feature importance - wrist-to-shoulder distance (basically how open your arms are)
is by far the strongest signal, which matches what you'd expect intuitively.

## What's not done yet
- There's a section in the notebook for extracting the same kind of features live from a webcam using
  MediaPipe, so the trained model could eventually work on a live video feed. Right now it's throwing
  an error (`mediapipe has no attribute 'solutions'`) that I still need to sort out - probably a
  version issue.
- Haven't checked yet whether my own MediaPipe feature calculations actually match the way the Kaggle
  dataset computed its features - need to verify that before the live demo can use this model properly.
- Still need to combine this with the facial emotion output from my teammate's part.