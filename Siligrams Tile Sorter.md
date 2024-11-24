### Project Scope
Build a tile sorter for the Siligrams mold tiles to enable faster sorting of tile inventory

**System Input** → Unsorted Non-oriented square and round tiles
**System Output** → Bins of tiles sorted by a range of categories
### Requirements
1. Reliable sorting and binning of tiles
2. Takes non-oriented tiles of all shapes and sizes
3. Gives tiles sorted into configurable categories
4. Hands off operation
5. Low error rate
6. Medium to High throughput
### Metrics

| Metric                    | Target                                  | Justification              |
| ------------------------- | --------------------------------------- | -------------------------- |
| Low Sorting Error Rate    | Less than 1 in every 20 tiles missorted | Need to have good accuracy |
| Medium to High Throughput | 60 tiles per minute                     | Arbitrary                           |
### Background Research

**Architecture Options**
- Due to the large variety of tiles, each with a unique pattern, an image matching optical sorter is probably the best bet
- For a given run of the sorter, the sorter does not need to 'know' that a given tile is A, B, or C. Instead it only needs to be able to distinguish that A is different from B is different from C. 
- This opens the door for open CV matching as opposed to using a trained image model 

[Optical Coffee Bean Sorter](https://edgeryders.eu/t/open-source-coffee-sorter-project/7122#chap-3-5) -> A high speed low cost sorter for coffee beans
[Fast FLANN with open cv](https://stackoverflow.com/questions/29563429/opencv-python-fast-way-to-match-a-picture-with-a-database) -> FLANN is a fast feature matching open CV approach
[Checking for image similarity with Open CV](https://stackoverflow.com/questions/11541154/checking-images-for-similarity-with-opencv) -> Discussion on image matching
[Rotation Agnostic Matching](https://stackoverflow.com/questions/10666436/scale-and-rotation-template-matching) 

[Guide to lighting for machine vision](https://www.advancedillumination.com/wp-content/uploads/2018/10/A-Practical-Guide-to-Machine-Vision-Lighting-v.-4-Generic.pdf)
[Bright field vs dark field](https://www.advancedillumination.com/lighting-education/bright-field-dark-field-lighting/)
[Details on darkfield illumination](https://www.vision-doctor.com/en/illumination-techniques/dark-field-illumination.html)

**Open CV validation**
Using feature matching, fairly consistent results seem to be achieved. It will be important to have a good clear photo for every shot
### Concept Generation