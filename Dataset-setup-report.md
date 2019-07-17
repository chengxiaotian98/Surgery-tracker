# Dataset Setup

Cheng
## Basic Steps:
- [X] Rename the videos 
- [X] Generate youtube-open-datasets/Images/ data, sampled from original video with fps=5, using `ffmpeg`
    - Note: if the duration of the video is longer than 10 min, then it is only sampled from the 10 min in the middle.
- [X] Generate metadata of Serena Video list
- [ ] Generate metadata of other Video list: <b> Dan's spreadsheet missed  this part </b>
- [X] Generate hand-detection-data 
    - Given the task that we should get N frames per video clip, we divide the folders, containing sampled-5fps-frames, into N parts. 
    - In each part, randomly choose one frame
    - Noted: I didn't choose to use `ffmpeg` to directly extract frames because it's too slow (about 1 frame/min, since it takes some time to traverse and find the frame) and would take days to have the data.
- [X] Evaluate the hand data and give out the exact frame number per video we should sample.
    - I randomly selected 10% part of the original data (except 'Plastic' and 'Vascular' since they respectively have 2 videos), and classify imgs to 'Good','So-so','Bad', 'Bad's are unqualified for labeling. Then I count the number of those classes and calculate the exact frames per video we should sample.
    - Assuming we would like 1500 frames to label, and given the fact that we have 460 videos('Serena dataset' excluded), I would give 2 values as suggestions: one is that we would like to have 1500 frames that are "Good", the other is that we want 1500 "Good" or "So-so" ones. The results are listed in the below. 

  frame per video (Good) | frame per video (Good or So-so)
  --- | ---
  6.62 | 5.15

* A little heads up, a qualified frame may contain multi-hands, so the labeling work could be greater than expected.
* The static sheet is uploaded in the Google Drive: [Link](https://drive.google.com/open?id=1wvbBPT-e1n7I8YGw-iul56Xoofqd49_W)


- [ ] Generate metadata for hand-detection-data

## Dataset

Type | Total on Youtube | Total Downloaded
--- | --- | --- 
[Gastrointestinal](https://www.youtube.com/playlist?list=PLjBdLTJUJ6w73P1fjPAtxYzExEk4t7FuP) | 210 | 184
[Head and Neck](https://www.youtube.com/playlist?list=PLjBdLTJUJ6w5MWji0p6vcS8TDsD8bPPc6) | 69 | 64 
[Breast](https://www.youtube.com/playlist?list=PLjBdLTJUJ6w50pwCgD9tVTam9L-FYdA2z) | 153 | 142
[Plastic](https://www.youtube.com/playlist?list=PLjBdLTJUJ6w6kF_CC7d-ZCuqTQt7mLvkI) | 2 | 2
[Vascular](https://www.youtube.com/playlist?list=PLjBdLTJUJ6w5GKF7h_-H4hrikbcoG6xeF) | 2 | 2
[Serena](https://www.youtube.com/playlist?list=PLegrqXHtHobDKdZDCcao5N9fweWrNIOej) | 74 | 66
Total (Serena excluded) | 436 | 394
Total | 510 | 460
 
