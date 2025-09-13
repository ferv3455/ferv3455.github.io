---
layout: home
title: "Research Experiences"
---

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
- Reproduced the algorithm of Active Learning by Feature Mixing (ALFA-Mix) based on our architecture, implemented a series of validation experiments on various standard machine learning datasets, and evaluated the proposed algorithm against it, producing results in favor of our method generally
- Proposed an improvement method for the clustering algorithm used in our method, yielding a training accuracy increased by 1-2% in certain training scenarios
- Assessed the performance of baseline algorithms and the new algorithm on datasets with the Average Rank method, proving that our proposed algorithm ranked 1st generally and not worse than 2nd in all tested cases


## Bi-level Optimization for Inductive Transfer Learning
<p>
    <div>
        Computational Biology Department, Carnegie Mellon University
        <br />
        Advisor: Min Xu, Associated Professor
        <div style="text-align: right;">August 2023 - July 2024</div>
    </div>
</p>

- Proposed a model pretraining method in which distinct sample weights are assigned to source data with a neural network learned with the DARTS method so that the pretrained model can achieve better performance when transferred to a specific, differently distributed dataset
- Located and fixed errors in the original code to ensure the successful reproduction of results for typical transfer learning tasks, and refactored the code to enable unified usage for different tasks
- Achieved satisfactory sample weight results by using subsets of MNIST, CIFAR-10 and CIFAR-100 datasets, which accurately reflected the correlation between source classes and target data
- Assessed the performance of the proposed method in specific scenarios where hybrid datasets with samples from multiple domains are used as source datasets, and produced an evident improvement of about 3% in accuracy with a limited target training dataset compared to baseline methods
- Combined the proposed method with SimCLR self-supervised learning architecture, and looked into the prospect of adopting sample weighting in the domain of self-supervised deep learning by conducting experiments with the CIFAR-10 dataset
- Explored the application of the proposed method to specialized datasets, including various medical and clinical datasets


## Log Data Reduction Algorithms Based on Provenance Graphs
<p>
    <div>
        School of Software, Tsinghua University
        <br />
        Advisor: Hai Wan, Associate Professor
        <div style="text-align: right;">November 2023 - June 2024</div>
    </div>
</p>

- Designed a complete framework for the evaluation of log data reduction algorithms based on provenance graphs in Python
- Implemented eight log data reduction algorithms proposed in the past decade based on papers and open-source projects
- Carried out experiments on real-world log data and compared the performance of these algorithms exhaustively


## Log Data Encoding for Efficient Storage in Apache IoTDB
<p>
    <div>
        School of Software, Tsinghua University
        <br />
        Advisor: Shaoxu Song, Associate Professor
        <div style="text-align: right;">October 2022 - May 2023</div>
    </div>
</p>

- Raised the concept of advanced character operations, and constructed the edit path of such operations from one string to another based on dynamic programming, which could be used as encoding information
- Added weights to different characters in the algorithm based on values correlated to frequencies such as Huffman codes, considering that commonly used characters could be compressed more efficiently
- Maintained the source string pool with a sliding window for selecting the one with the shortest distance
- Classified the assortment of log data according to data formats by applying clustering algorithms, minimizing necessary data storage
- Handled the temporal optimization of the algorithm by adopting an approximate string distance algorithm, Cosine Distance based on Q-grams, with linear time complexity


## Developing a Network Security Data Management System
<p>
    <div>
        School of Software, Tsinghua University
        <br />
        Advisor: Hai Wan, Associate Professor
        <div style="text-align: right;">October 2023 - December 2023</div>
    </div>
</p>

- Designed and improved the user login interface and its functions, including registration, login, logout, personal information management, and resetting the password
- Participated in the collection and maintenance of the attack behavior dataset, and the generation and maintenance of the provenance graph dataset
- Implemented the algorithm details page, including the algorithm introduction, model assessment and code submission, and designed the navigation bar of the web page
- Designed test cases in the functional and end-to-end tests of the web application


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
- Tuned the existing model for the DVS dataset to achieve an improvement in the classification accuracy
- Constructed a new model based on the GNN framework and fitted it to a simplified subset of the dataset
- Attempted to extend the dataset, and tried to optimize the network model based on the GNN framework in order to improve the classification accuracy
