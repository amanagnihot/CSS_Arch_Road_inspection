3-02-26- Road Defect Segmentation: U-Net+ResNet18 vs Depthwise-Separable Custom Architecture | PyTorch, Albumentations, COCO Format | Baseline Comparison (14M→1M params, 90%+ accuracy retention) for Edge Deployment

04-02-2026	Modelling	Aman	Road Defect Detection: RT-DETR-R18 vs RoadDefectNet-Lite | PyTorch, YOLOv11 Format, MobileNetV3-Small Backbone	Baseline Comparison (28M→2.3M params, 12x lighter, 3x faster) for Jetson Nano/Drone Edge Deployment

05-02-2026 Modelling Aman Distilation of RTDETR-R18 and improvement of Custom mdoel architecture , paper review Loss function was just learning to detect no object so it is performing better in map but when testing on real world it is failing

06-02-2026  Modelling Aman Updated the custom architecture and improved the threshold for testing detection and updated the data set by removing unbalanced class improved data balancing improves training threshold value is reduced to remove zero precision 

09-02-2026	Modelling	Aman	Built a lightweight 0.69M parameter detection model using Depthwise Separable Convolutions and MobileNetV3 blocks, 	Optimized for Raspberry Pi edge deployment with 30+ FPS inference
09-02-26	Modelling	Aman	Integrated a 3-scale Feature Pyramid Network (FPN), Implemented gradient accumulation during training ,	Enables simultaneous detection of small cracks and large potholes in the same image, Allows full model training on limited GPU memory without quality loss
09-02-26	Data cleaing 	Aman	Trained model on 1,582 images across 5 filtered road defect classes , Ran raw model diagnostics after observing 0% precision 	Model learns to detect Crack, Edge_breaking, Guard_stone, Ravelling, and Pothole, Discovered model was detecting objects but with confidence scores far below the set threshold 
09-02-26	Modelling	Aman	Performed root cause analysis on the detection pipeline	Identified confidence threshold mismatch (0.25 set vs 0.01–0.09 actual output) as the sole failure point
09-02-26	Modelling	Aman	 Instead of heavy deep learning models (YOLO/R-CNN), we use classical Machine Learning (XGBoost/LightGBM) trained on handcrafted image features (HOG, LBP, Texture, Edges) extracted from annotated road defect images — making the model lightweight, fast, and deployable on edge devices without GPU.	Trained on 7,892 samples across 5 defect classes using Google Colab T4 GPU, achieving 84.6% accuracy with a 2.4MB model running at 0.09ms inference speed — speed and size targets met, working on pushing accuracy from 84.6% to 90%+ using ensemble methods.
10-02-2026	Modelling	Aman	Diagnosed root causes of 0% precision across three pipeline components — hardcoded grid size (13 vs actual 14), missing sigmoid on box coordinate decode, and 416×416 input shrinking 4K drone objects to 7px minimum 	Identified exactly why the model produced zero detections despite clean labels, correct gradients, and working loss function
10-02-2026	Modelling	Aman	Rebuilt detection pipeline with 640×640 input, dual-scale heads (80×80 + 20×20), dynamic grid size, and expanded test set from 6 to 106 images	Ensured objects remain 12px+ minimum size, both small and large defects are detectable, and evaluation results are statistically reliable
10-02-26	Data cleaing 	Aman	Per-Class Dataset Split	Split parent dataset into 15 individual binary datasets (one per defect class) with positive + balanced negative samples, saved to Google Drive
10-02-26	Modelling	Aman	Instead of one 5-class model,	 training one binary model per defect class (Yes/No detector) — runs all simultaneously at inference for better accuracy and modularity

