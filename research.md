---
layout: homepage
---

# Rearch Experiences

## Deep Active Learning with Noise Stability
<p>
    <div>
        Computational Biology Department, Carnegie Mellon University
        <br />
        Advisor: Min Xu, Associated Professor
        <div style="text-align: right;">July 2023 - August 2023</div>
    </div>
</p>

- Participated in proposing and developing a novel algorithm leveraging noise stability to estimate prediction uncertainty for unlabeled data, in which random noises are added to model parameters, and the consequent perturbation of output is regarded as model prediction stability
- Conducted extensive theoretical analysis using the small Gaussian noise theory, and proved that the novel algorithm favors subsets with large and diversified gradients
- Reproduced the algorithm of Active Learning by Feature Mixing (ALFA-Mix) based on our architecture, implemented a series of validation experiments on various standard machine learning datasets, and evaluated the proposed algorithm against it, producing results in favor of our method
- Proposed an improvement method for the clustering algorithm used in our method, yielding a training accuracy increased by 1-2% in certain training scenarios
- Assessed the performance of different baseline algorithms and the new algorithm on different datasets using the Average Rank method, demonstrating that our proposed algorithm ranked 1st generally and not worse than 2nd in all tested cases

---

## Bi-level Optimization for Inductive Transfer Learning
<p>
    <div>
        Computational Biology Department, Carnegie Mellon University
        <br />
        Advisor: Min Xu, Associated Professor
        <div style="text-align: right;">August 2023 - Present</div>
    </div>
</p>

- Proposed a model pretraining method in which distinct sample weights are assigned to source data with a neural network so that the pretrained model can achieve better performance specifically when transferred to a differently distributed dataset
- Located and fixed errors in the original code to ensure the successful reproduction of results for typical transfer learning tasks, and refactored the code to enable unified usage for different tasks
- Achieved satisfactory sample weight results by using subsets of MNIST, CIFAR-10 and CIFAR-100 datasets, which accurately reflected the correlation between source classes and target data
- Assessed the performance of the proposed method in specific scenarios where hybrid datasets with samples of similar classes from multiple domains are used as source datasets, and produced an evident improvement of about 3% in accuracy with a limited target training dataset compared to baseline methods
- Combined the proposed method with SimCLR self-supervised learning architecture, and looked into the prospect of adopting sample weighting in the domain of self-supervised deep learning by conducting experiments with the CIFAR-10 dataset
- Explored the application of the proposed method to specialized datasets, including various medical and clinical datasets

---

## Log Data Encoding for Efficient Storage in Apache IoTDB
<p>
    <div>
        School of Software, Tsinghua University
        <br />
        Advisor: Shaoxu Song, Associated Professor
        <div style="text-align: right;">October 2022 - May 2023</div>
    </div>
</p>

- Proposed a string encoding method based on edit distance aimed at saving log message data produced by Apache IoTDB databases in the most efficient way
- Raised the concept of advanced character operations, and constructed the edit path of such operations from one string to another based on dynamic programming, which could be used as encoding information 
- Selected the source string pool with a sliding window and thereby encoded the entire log file efficiently
- Classified the assortment of log data into groups of different data formats by applying clustering algorithms, which minimized necessary data storage
- Investigated the temporal optimization of the algorithm by adopting an approximate string distance algorithm with linear time complexity

---

## Developing a Network Security Data Management System
<p>
    <div>
        School of Software, Tsinghua University
        <br />
        Advisor: Hai Wang, Associate Research Fellow
        <div style="text-align: right;">October 2023 - Present</div>
    </div>
</p>

- Designed and improved the user login interface and its functions, including registration, login, logout, personal information management, password change, and forgetting the password
- Participated in the acquisition and maintenance of the attack behavior data set, and the generation and maintenance of the traceability graph data set
- Implemented the development of the algorithm details page, including the algorithm introduction, evaluation history, model upload, submission and evaluation, and designed the interface in the upper column of the web page
- Completed the docking of the back-end interface, and implemented the test experiment

---

## Visual Perception and Understanding of Three-dimensional Scenes: Action Recognition
<p>
    <div>
        School of Software, Tsinghua University
        <br />
        Advisor: Yue Gao, Associate Professor
        <div style="text-align: right;">October 2021 - September 2022</div>
    </div>
</p>

- Aimed to build a DVS action dataset and implement methods for action recognition through DVS video
- Collected the largest DVS dataset of 50 actions with 2330 segments
- Tuned the existing model for the DVS dataset to achieve an improvement in classification accuracy
- Constructed a new model based on GNN framework and fitted it to a simplified subset of the dataset
- Attempted to expand the dataset, and tried to optimize the network model based on the GNN framework in order to improve the recognition accuracy
