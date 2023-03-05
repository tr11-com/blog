---
title: "How does Machine Learning actually work?"
datePublished: Sun Feb 19 2023 10:36:39 GMT+0000 (Coordinated Universal Time)
cuid: cleb97mvf000309mdehsf4evy
slug: how-does-machine-learning-actually-work
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/ZJKE4XVlKIA/upload/86eae459ea52283575389249b17fd719.jpeg
tags: artificial-intelligence, machine-learning

---

Greetings, fellow humans! Today, we'll delve into the fascinating world of Machine Learning and learn how it works. But don't worry, we won't make it as dry as a calculus lecture. So, let's get started!

### **What is Machine Learning?**

Machine Learning is a type of Artificial Intelligence that allows machines to learn from data without being explicitly programmed. In essence, we're trying to teach machines to learn from experience, similar to how we humans do it.

There are three primary types of Machine Learning: **Supervised Learning**, **Unsupervised Learning**, and **Reinforcement Learning**.

1. ### Supervised Learning
    

Supervised Learning is the type of Machine Learning where we train machines on labeled data. The machine learns to map input data to output data by identifying patterns in the data. A classic example of supervised learning is image classification, where the machine is trained to recognize images and label them correctly.

Here's an example of how you can train a machine to classify images of cats and dogs using the popular TensorFlow library:

```python
import tensorflow as tf

# Load dataset
(train_images, train_labels), (test_images, test_labels) = tf.keras.datasets.cifar10.load_data()

# Define model architecture
model = tf.keras.models.Sequential([
  tf.keras.layers.Conv2D(32, (3,3), activation='relu', input_shape=(32, 32, 3)),
  tf.keras.layers.MaxPooling2D((2,2)),
  tf.keras.layers.Flatten(),
  tf.keras.layers.Dense(64, activation='relu'),
  tf.keras.layers.Dense(10, activation='softmax')
])

# Compile model
model.compile(optimizer='adam', loss='sparse_categorical_crossentropy', metrics=['accuracy'])

# Train model
model.fit(train_images, train_labels, epochs=10)

# Evaluate model
test_loss, test_acc = model.evaluate(test_images, test_labels)
print("Test accuracy: ", test_acc)
```

1. ### Unsupervised Learning
    

Unsupervised Learning is where machines learn from unlabeled data. The machine tries to find patterns and structure in the data without any predefined labels. A common application of unsupervised learning is clustering, where the machine groups similar data points together.

### **Reinforcement Learning**

Reinforcement Learning is a type of Machine Learning where machines learn through trial and error. The machine interacts with an environment and receives rewards or punishments based on its actions. The goal is for the machine to learn the optimal set of actions to take to maximize the reward.

An example of Reinforcement Learning is training a machine to play a game, like tic-tac-toe.

![](https://kroki.io/mermaid/svg/eNpLL0osyFDwCeJyjHZJTcvMS1VITE_NK1FIzEtRSE_MTVVIzSvLLMrPywUKxiro6topOGmEFCVm5kHUaXI5gQWdNUJSi0tgetOB8kBeRmluYp5CQU5iZWqRJhcAZp8jRg align="center")

Thanks for reading!