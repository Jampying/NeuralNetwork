Project Overview: This Neural Network catergorizes and predicts price trends for the following day using the previous weeks data. These predictions are for the price of Alibaba Stock. The dataset tracks prices daily and includes the following points: date, open, close, high, low, adjusted close, and volume. The goal is to determine which past indicator is the most indicative of future trends.

**note the vast majority of the model portion of the code is directly from the neural networks from scratch textbook, there are only so many ways you can write the chainrule, ReLU, layer initialization etc however the data module is solely mine**

Project Decision: I have been working with neural networks and financial modeling separately
for a while now, beginning in 2022 with the Nvidia workshop, I figured I would like to understand
the logic behind the magic and, hopefully, use this increased knowledge to further optimize and
perfect my own private networks.

How to Run: 
1. Navigate to codio.com
2. My Projects > New Project > python > create
3. file > upload > NeuralNetwork.py
4. file > upload > DataSlidingRule.py
5. file > upload > {your dataset name}.csv


Instructions on use:
1. grab historical pricing for a stock ensuring the high and low columns are present
2. run the DataSlidingRule.py file
3. in the DataSlidingRule.py file call the average() function with name of desired column to have its' average added to the new dataset
4. when all desired columns are added run the direction() function to generated the ytrue values in one hot vector format
5. then run the normalize() function to normalize your data points
6. then run the output() function to save your ytrue and normalized datasets as y.csv and X.csv respectively
7. go to neural network.py and create a new layer dense instance name "dense1" of the same shape as your dataset's second dimension (ex. inputs = [1840, 4], layerDense(4, 20))
8. make as many hidden layers as you would like matching the shape of the second dimension with the first of the subsequent layer (ex. dense1 = [4, 20] dense2 = [20, 69])
9. instantiate the first activation fucntion, the loss function, the accuracy class, and the optimizer
    **Note: the following should all be done within a for loop that has the number of desired epochs as its range**
11. run the forward pass of each layer of the network, layer one uses the normalized dataset as its input every subsequent layer uses the previous layer's activation function output (ex. dense1.forward(X), activation1.forward(dense1.output), dense2.forward(activation1.output)
12. for the final dense layer run the forward pass then run the loss function (ex. dense2.forward(activation1.output), lossActivationFunction.forward(dense2.output))
13. run the backward passes in the reverse order of the forward, starting with the loss and moving back to the backward pass of the first dense layer
14. run the optimizer preParamUpdate() function
15. run the optimizer paramUpdate() function
16. run the optimizer postParamUpdate() function

Specific Course Concepts used:
1. Expression Parsing
2. Encapsulation
3. Classes/Instances
4 .Class Inheritance
5. Python
6. Parallel Computing
7. Functions and Parameters
8. Paradigms and Data Types

Favorite Chunk of Code: This code uses the sliding window algorithim to find the average of a weeks worth of traded stock prices, add them to a queue to make it more runtime efficient, average the queue
and add the averaged value to a new dataframe to be normalized later

    def average(self, header):
        #initialize the queue for k values
        queue = []
        newDataSet[header] = ""
        #checks k is in the bounds of the dataset
        if(self.k - 1 < dataset[header].shape[0]):
            #initializes the queue with the first k data points
            for i in range(self.k):
                queue.append(dataset[header].at[i])
        #initializes the first pointer to zero
        self.p1 = 0
        #loops through all values of the provided column
        while(True):
            #averages the queue
            queueAverage = sum(queue) / len(queue)
            #adds the obtained average to the new dataset
            newDataSet.loc[self.p1,header] = queueAverage
            #increases the first pointer by one
            self.p1 += 1
            #sets the second pointer to pointer 1 index offset by k
            self.p2 = self.p1 + self.k
            #Compares the second pointer to the shape of the rows and breaks if the same
            if self.p2 == dataset.shape[0]:
                print(f"Pointer 2 is size {self.p2}")
                break
            #removes the first element of the queue
            queue.pop()
            #adds a new value at the second pointer
            queue.append(dataset[header].at[self.p2])
        #displays new dataset to be observed for error
        print(newDataSet)

    
