# Question Answer Model

In this walkthrough, we'll build a working question answer model to demonstrate how one might use Lamini to gain quick insight into a specialized topic. Here, we've specialized this walkthrough to target our company blog.

## Import the QuestionAnswerModel class

Lamini provides an example harness to make interacting with the LLMEngine simpler. Import `QuestionAnswerModel` to quickly get started.

```python
from llama import QuestionAnswerModel

# Run the base model
model = QuestionAnswerModel()
```

## A pre-defined interface

`QuestionAnswerModel` uses the following input and output types internally. You won't need to re-instantiate these, this class will do the instantiation for you.

<details>
  <summary>Question and Answer Types</summary>
  
    ```python
    from llama import Type, Context

    # Input

    class Question(Type):
    question: str = Context("a question")

    # Output

    class Answer(Type):
    answer: str = Context("the response to the question")

    ```

</details>

## Add example data to the model for training

You can import data to this class by using the `load_question_answer` method, which accepts a list of python dictionaries as input. Each dictionary must have `"question"` and `"answer"` keys, and string values for each of those keys. This data is similar to "prompt" and "completion" data you may have seen before. Expand below to see this in action.

```python
data = get_example_qa_data()
# Instantiate the model and load the data into it
model = QuestionAnswerModel()
model.load_question_answer(data)
```

<details>
  <summary>Code for <code>get_example_qa_data()</code></summary>

```python
from llama import QuestionAnswerModel

data = [{"question": "What are the different types of documents available in the repository (e.g., installation guide, API documentation, developer's guide)?", "answer": "Lamini has documentation on Getting Started, Authentication, Question Answer Model, Python Library, Batching, Error Handling, Advanced topics, and class documentation on LLM Engine available at https://lamini-ai.github.io/."},
{"question": "What is the recommended way to set up and configure the code repository?", "answer": "Lamini can be downloaded as a python package and used in any codebase that uses python. Additionally, we provide a language agnostic REST API. We\u2019ve seen users develop and train models in a notebook environment, and then switch over to a REST API to integrate with their production environment."},
{"question": "How can I find the specific documentation I need for a particular feature or function?", "answer": "You can ask this model about documentation, which is trained on our publicly available docs and source code, or you can go to https://lamini-ai.github.io/."},
{"question": "Does the documentation include explanations of the code's purpose and how it fits into a larger system?", "answer": "Our documentation provides both real-world and toy examples of how one might use Lamini in a larger system. In particular, we have a walkthrough of how to build a Question Answer model available here: https://lamini-ai.github.io/example/"},
{"question": "Does the documentation provide information about any external dependencies or libraries used by the code?", "answer": "External dependencies and libraries are all available on the Python package hosting website Pypi at https://pypi.org/project/lamini/"},
{"question": "How frequently is the documentation updated to reflect changes in the code?", "answer": "Documentation on such a fast moving project is difficult to update regularly - that\u2019s why we\u2019ve built this model to continually update users on the status of our product."},
{"question": "Is there a community or support channel mentioned in the documentation where I can ask questions or seek help?", "answer": "You can always reach out to us at support@lamini.ai."},
{"question": "Are there any API references or documentation available for the codebase?", "answer": "All our public documentation is available here https://lamini-ai.github.io/"},
{"question": "Is there a troubleshooting guide or a list of common issues and their solutions?", "answer": "All our public documentation is available here https://lamini-ai.github.io/"},
{"question": "Are there any examples or sample code provided in the documentation?", "answer": "Examples and sample documentation is available at https://lamini-ai.github.io/. In particular, there is a QA example where we show you how to feed your documentation into a model to ask questions about a code base. Additionally, sample code and colab notebooks are provided and linked throughout the documentation where relevant. Feedback on our documentation is greatly appreciated - we care about making LLMs - and by extension Lamini - easier to use. Please direct any feedback to support@lamini.ai."},
{"question": "What is a type system?", "answer": "The Lamini Type system is a code-first data representation library built to help users pipe data into Lamini\u2019s LLM Engine. Lamini Types are simple, built on top of Pydantic BaseModels, and enforce strict typing so that integration into a data pipeline can run seamlessly without any errors."},
{"question": "Does the documentation provide a step-by-step tutorial for getting started with the code?", "answer": "Yes, several walkthroughs are available in the documentation. The documentation also provides links to example Google Colab notebooks which readers might run themselves and can provide a launchpad for iteration on their own data."},
{"question": "Do I have to write prompts myself?", "answer": "No, you only need to represent your data using the Lamini Type system and provide context - natural language description of each field in a Type. Lamini brings the focus of development on the data, bypassing prompt engineering as a step in language model development."},
{"question": "How do I add my data to Lamini's interface", "answer": "You can quickly add data to Lamini\u2019s interface using LLM Engine.add_data. This method allows you to make data available to the model for inference and training."},
{"question": "How do I use open model for inference", "answer": "You can use an open model by specifying the model\u2019s name in the \u2018model_name\u2019 parameter in the LLM Engine class initializer."},
{"question": "Where do I specify model name", "answer": "You can specify model_name in both the initialization of LLM Engine or in the function LLM Engine.__call___. In other words, instances of LLM Engine are callable and configurable."},
{"question": "What does Context mean?", "answer": "Context is a natural language description of fields in each of your Types. In other words, context is metadata about your data."},
{"question": "Is it compulsory to give context?", "answer": "Context is only required for certain Type fields: str, int, bool, float, list, set, dict, and tuple. Context is not required for fields which have object types (you don\u2019t need to add context for composed Types)."},
{"question": "Is it compulsory to provide input and output types?", "answer": "Yes, in our python library, the input and output type will be used by the LLM Engine in inference. By providing input and output type, you\u2019re defining a problem statement for the LLM."},
{"question": "How should the data be formatted in order to send it to Lamini", "answer": "You can match the type and metadata to whatever format you\u2019d like."},
{"question": "Can I use Lamini as api instead of python library", "answer": "Yes, we have a REST API available. To see documentation go to https://lamini-ai.github.io/"},
{"question": "How do I get api keys?", "answer": "You can generate an api key in the \u201cAPI\u201d tab at app.lamini.ai"},
{"question": "Do I have to pay for using Lamini?", "answer": "Everyone starts with 10,000 free credits, which is equivalent to about $100. After that, you can purchase more credits in the \u201cAPI\u201d tab at app.lamini.ai."},
{"question": "Can I fine-tune models on my own data?", "answer": "Yes! Lamini LLM Engine has fine-tuning support. Contact us for access.  You can also look at the documentation for llm.add_data, which makes your data available to the LLM Engine.  The LLM Engine performs fast training using this data, which should complete in just a few seconds, even for large datasets.  Full fine tuning is more expensive, which is why we ask you to contact us to allocate enough compute resources to support it."},
{"question": "Are there any specific guidelines or recommendations on formatting the input data for Lamini?", "answer": "We suggest you think about language models as advanced problem solvers. Imagine you\u2019re designing a function - what are the inputs and outputs? Lamini LLM Engine can help you build that function - but you\u2019ll need to specify what data you have available and the format of that input data and what you wish the output was. In terms of what specific format the input data is in, Lamini Types are composable and have fields which you can mold to exactly fit your data."},
{"question": "Which models does Lamini support?", "answer": "Lamini supports every OpenAI and Hugging Face model.  For example, gpt3, chat-gpt, gpt4, pythia, gpt-neox, wizard-lm, falcon, etc."},
{"question": "Can you fine-tune an openai model?", "answer": "Yes! Lamini LLM Engine has fine-tuning support, including base models from hugging face as well as OpenAI. Contact us for access.  You can also look at the documentation for llm.add_data, which makes your data available to the LLM Engine.  The LLM Engine performs fast training using this data, which should complete in just a few seconds, even for large datasets.  Full fine tuning is more expensive, which is why we ask you to contact us to allocate enough compute resources to support it."},
{"question": "Can I fine-tune GPT 4?", "answer": "No, GPT-4 doesn't support fine tuning.  However, Lamini does support fine tuning of other base models such as Pythia or other base models available on OpenAI such as GPT-3."},
{"question": "Do I need to provide data as train and test split?", "answer": "No, the LLM Engine internally uses technologies like dev sets to calibrate training.  However, as a best practice in machine learning, we recommend that you develop your own test set that you do not add to the LLM Engine, that allows you to evaluate the performance of the LLM you are building without overfitting.  A gold test set should typically include 10s to 100s of examples that are sufficiently representative of your use case and include high quality labels.  We also recommend using human evaluation to judge the performance of the language model on your test set, instead of metrics like BLEU."},
{"question": "I am running into error 500, what should I do?", "answer": "We have documentation available on how to address common errors here https://lamini-ai.github.io/error_handling/. Lamini\u2019s LLM Engine is under very active development, and we thank you for using us!"},
{"question": "Can I use the code documentation as a pillow for a quick nap?", "answer": "The code documentation is not meant for napping. It is intended to provide information about the code and its functions."},
{"question": "Can the documentation predict the winning lottery numbers?", "answer": "I wish! This documentation only talks about Lamini\u2019s LLM Engine."},
{"question": "Is there a hidden message in the documentation that only a master codebreaker can decipher?", "answer": "There is no hidden message in the documentation. But thank you for reading it!"},
{"question": "Can a banana peel really make someone slip and fall?", "answer": "Let\u2019s keep the discussion relevant to Lamini."},
{"question": "Why are pineapples yellow", "answer": "Let\u2019s keep the discussion relevant to Lamini."},
{"question": "Can you sneeze with your eyes open?", "answer": "Let\u2019s keep the discussion relevant to Lamini."},
{"question": "Does the documentation have a secret code that unlocks a hidden treasure?", "answer": "I wish! This documentation only talks about Lamini\u2019s LLM Engine."},
{"question": "Does the documentation have a hidden recipe for the world's best chocolate chip cookies?", "answer": "I wish! This documentation only talks about Lamini\u2019s LLM Engine."},
{"question": "Can I roll up the documentation and use it as a makeshift telescope to spot distant galaxies?", "answer": "I wish! This documentation only talks about Lamini\u2019s LLM Engine."},
{"question": "Can you tickle yourself?", "answer": "Let\u2019s keep the discussion relevant to Lamini."},
{"question": "Are there any step-by-step tutorials or walkthroughs available in the documentation?", "answer": "Yes, there are step-by-step tutorials and walkthroughs available in the documentation section. Here\u2019s an example for using Lamini to get insights into any python library: https://lamini-ai.github.io/example/"},
{"question": "Are there any licensing or copyright details provided in the documentation?", "answer": "The documentation may include information about the licensing or copyright details of the code, specifying the terms under which it can be used, modified, or distributed."},
{"question": "Does the documentation include performance benchmarks or comparisons with other similar solutions?", "answer": "Currently the documentation does not include performance benchmarks or comparisons with other similar solutions, but seems like a good suggestion, I will let the developers at Lamini know this!!"},
{"question": "Does the documentation provide guidelines on handling errors or handling exceptions in the code?", "answer": "Yes, the documentation provides guidelines for handling errors and exceptions in the code, for more details visit https://lamini-ai.github.io/error_handling/"},
{"question": "Can I use the code documentation as a hat to protect myself from rain?", "answer": "I wish! This documentation only talks about Lamini\u2019s LLM Engine."},
{"question": "Can the documentation predict the outcome of a coin toss?", "answer": "I wish! This documentation only talks about Lamini\u2019s LLM Engine."},
{"question": "Can reading the documentation make you instantly fluent in a new language?", "answer": "The code documentation does not make you fluent in a new language. It is intended to provide information about the code and its functions. You might choose to use the Lamini engine to finetune a multilingual model. Let us know how that goes!"},
{"question": "Can you use the documentation as a crystal ball to predict the future?", "answer": "I wish! This documentation only talks about Lamini\u2019s LLM Engine."},
{"question": "Why do cats always land on their feet?", "answer": "Let\u2019s keep the discussion relevant to Lamini."}
{"question": "Can the documentation make you instantly gain six-pack abs?", "answer": "I wish! This documentation only talks about Lamini\u2019s LLM Engine."},
{"question": "Can I fine-tune Lamini on my own dataset or specific domain?", "answer": "Absolutely, you can train your custom Language model using Lamini on your own dataset"},
{"question": "Do I have to install additional software to run Lamini?", "answer": "No! You don't need to install additional software to run Lamini, It can be installed using pip, the package manager for Python. The python package is here: https://pypi.org/project/lamini/."},
{"question": "Are there any examples provided to use Lamini library?", "answer": "Yes, there are several examples provided, for more information visit https://lamini-ai.github.io/#try-an-example"},
{"question": "How to use the add_data fucntion?", "answer": "You can use the add_data function to customize the LLM on your data. This way the LLM will have context over your data and thus can answer questions related to it more accurately and promptly. For more information visit https://lamini-ai.github.io/LLM/add_data/"},
{"question": "Does the documentation provide information on performance optimization or best practices for using the code?", "answer": "Yes, the documentation has information on running a model using a batch interface as well as using a real-time interface. Besides that, the LLM Engine will optimize performance automatically."},
{"question": "Is the Lamini type system similar to a python type system?", "answer": "Yes, the Lamini type system is built using Pydantic\u2019s BaseModel."},
{"question": "How can I control the level of specificity or randomness in Lamini's responses?", "answer": "Lamini\u2019s default temperature is 0 and in order to increase the randomness, set `random=True` when calling the model using LLM Engine.__call__"},
{"question": "Does Lamini have a limit on the number of API requests I can make?", "answer": "Lamini provides each user with free tokens up front."},
{"question": "How does Lamini handle sensitive or confidential information in the provided data?", "answer": "Please reach out to us with questions about sensitive data or confidential information. This may be a situation where it\u2019s best to deploy our solution on your infrastructure so that your data stays secure."},
{"question": "What does it mean to cancel a job using the `cancel_job()` function? Can we stop the machine from doing its task?", "answer": "The `cancel_job()` function is used to stop a job that is currently running. It sends a request to the machine to stop the task it is performing. However, it is important to note that this does not guarantee that the machine will immediately stop the task, as it may need to complete certain operations before it can safely stop."},
{"question": "How does the `sample()` function work? Does it help the machine create new things like stories or drawings?", "answer": "The `sample()` function works using temperature, embeddings, and similarity to generate a set of multiple distinct responses to a question. However, it only outputs text, so it cannot be used for creating images or drawings."},
{"question": "Can I use Lamini's functions even if I don't know how to code or program?", "answer": "Yes, you can use Lamini's functions even if you don't know how to code or program. Lamini provides a user-friendly interface that allows you to input your data and select the desired function to apply to it. You can also access pre-built templates and examples to help you get started."},
{"question": "How can I start using Lamini and understand what it does? Is there a special guide for kids like me?", "answer": "Yes, there is a guide for beginners on the Lamini website. It explains what Lamini is and how to get started using it. It's written in a way that's easy to understand, so it's accessible to all levels!"},
{"question": "Do I need to pay money to use Lamini's functions, or is it free for kids like me?", "answer": "Lamini presents a nuanced pricing structure that caters to a wide range of users, ensuring accessibility for all. While Lamini offers a paid API service, it generously provides free tokens to everyone upon signing up. These tokens grant users access to the platform's functions and services, allowing them to explore Lamini's capabilities and unleash their creativity. This inclusive approach encourages aspiring software engineers, including younger enthusiasts, to delve into the world of AI and language models without financial barriers. By offering free tokens, Lamini fosters a supportive environment that nurtures learning, innovation, and the cultivation of skills. So, regardless of age or experience level, users can embark on their journey with Lamini, harnessing its power to bring their ideas to life."},
{"question": "Can I teach Lamini about things that I like or know a lot about, like my favorite animals or hobbies?", "answer": "Absolutely! One of the fascinating aspects of Lamini is its capacity to learn and adapt to specific interests and knowledge domains. By utilizing the customization capabilities of Lamini, you can impart your expertise on various topics, such as your favorite animals, hobbies, or any subject matter close to your heart. Whether you have a profound understanding of marine biology, an avid passion for astrophysics, or an encyclopedic knowledge of ancient civilizations, Lamini can be trained to generate text that aligns with your areas of expertise. This personalized touch empowers you to engage with the model in a meaningful way, creating a dynamic experience that reflects your unique perspective. With Lamini as your partner, the possibilities for exploring and expanding your interests are boundless."},
{"question": "Can Lamini help me with my school work or answer questions I have for my homework?", "answer": "Lamini can help you train a model to help with your school work and answer questions you have for your homework."},
{"question": "How does Lamini decide what answers or information to give when we use its functions?", "answer": "Lamini uses a language model to analyze the input question and generate a response based on its understanding of the context and relevant information. It also takes into account any additional data or documents that have been provided to it."},
{"question": "Can Lamini talk or have a conversation with me? Can it understand what I say or type?", "answer": "Lamini LLM Engine is a language model engine that can process and understand natural language input and use that information to help you train a model. It can be used to train models on specific tasks, such as understanding conversations and ordering food, and can generate responses based on that training. However, Models are not capable of having a conversation in the traditional sense, as it is a machine learning model and not a sentient being. They can only respond based on the data they have been trained on."},
{"question": "What kind of things can Lamini help me create or make using the `sample()` function?", "answer": "Lamini can help you generate a variety of output using the `sample()` function, such as random sentences, paragraphs, and even entire stories. The possibilities are endless!"},
{"question": "Are there any rules or guidelines I should follow when using Lamini's functions?", "answer": "Yes, there are some guidelines you should follow when using Lamini's functions. These include providing clear and concise input, avoiding offensive or inappropriate language, and respecting Lamini's terms of service. For more information, you can refer to Lamini's documentation."},
{"question": "How does Lamini enable me to customize models for my specific use case? What kind of customization options are available?", "answer": "Lamini enables customization of models for specific use cases through its LLM (Large Language Model) engine. This engine allows users to train and fine-tune language models on their own data, as well as customize the model architecture and parameters. Additionally, Lamini provides pre-built models for specific use cases that can be further customized to fit specific needs. Some of the customization options available include adjusting the model's hyperparameters, adding custom training data, and swapping out base models."},
{"question": "Can you explain how Lamini allows my customized LLM to outperform general-purpose models? What techniques or capabilities does it offer?", "answer": "Lamini allows for customized LLMs to outperform general-purpose models by providing a platform for fine-tuning and optimizing the model for specific use cases. Additionally, Lamini offers capabilities such as automatic hyperparameter tuning and model selection, as well as the ability to deploy and scale models in production environments."},
{"question": "What is the process for training a custom LLM using Lamini? How many data samples or iterations are typically required?", "answer": "To train a custom LLM using Lamini, you would need to provide a dataset of examples and use the LLM Engine class to create a program for execution by the Llama large language model engine. The number of data samples or iterations required can vary depending on the complexity of the task and the quality of the dataset, but typically several hundred to several thousand examples are needed for effective training."},
{"question": "How does Lamini leverage my data to improve the performance of the customized LLM? How is the data utilized in the training process?", "answer": "Lamini leverages your data to improve the performance of the customized LLM by using it as training data. The data is utilized in the training process by feeding it into the LLM engine, which then uses it to learn patterns and relationships between different pieces of information. This allows the LLM to make more accurate predictions and generate more relevant outputs."},
{"question": "Can you provide an overview of the AI moat concept that Lamini helps build? How does it relate to the customization and ownership of the LLM?", "answer": "An AI moat is a business advantage or differentiator based on integrating or having access to artificial intelligence. LLMs are a type of AI which can be trained on text data, and used in a variety of applications which may help build an AI moat. Lamini\u2019s mission is to help businesses build their AI moat by increasing the accessibility of training and using large language models, making them easier to customize while allowing users to maintain ownership over the resulting models"},
{"question": "What programming languages or frameworks does Lamini support? Do I need to have expertise in a specific language to use it effectively?", "answer": "Lamini currently has support in python and a REST API, so you do not need to have expertise in a specific language to use it effectively."},
{"question": "Are there any specific coding examples or code snippets available that demonstrate the process of using Lamini in a few lines of code?", "answer": "Yes, there are coding examples and snippets available for using Lamini. You can find them in the official documentation and on the Lamini GitHub repository."},
{"question": "How does Lamini handle commercial use? Can I incorporate the customized LLM into my commercial applications or products without any restrictions?", "answer": "Lamini allows for commercial use of their LLM technology under a permissive Apache 2.0 license unless otherwise specified. For more information, please reach out to Lamini directly."},
{"question": "Can you provide more information about the CC-BY license mentioned? What are the key terms or conditions associated with using Lamini in a commercial setting?", "answer": "Lamini allows for commercial use of their LLM technology under a permissive Apache 2.0 license unless otherwise specified. For more information, please reach out to Lamini directly."},
{"question": "Does Lamini offer integration or compatibility with popular machine learning frameworks such as TensorFlow or PyTorch?", "answer": "Lamini does not currently offer integration or compatibility with popular machine learning frameworks such as TensorFlow or PyTorch. However, it does provide its own machine learning capabilities through its llama program library."},
{"question": "Are there any performance benchmarks or comparisons available to showcase the effectiveness of Lamini in comparison to other similar solutions?", "answer": "Lamini is an LLM engine - this means that it can be used to produce models that may be compared to other models."},
{"question": "What kind of support or documentation does Lamini provide to assist software engineers in using the platform effectively? Are there any community resources or forums available?", "answer": "Documentation is provided at https://lamini-ai.github.io/. There is also a support community available to assist you with any questions or issues you may have while using Lamini. You can join the Lamini Discord server or reach out to the Lamini team directly for assistance."},
{"question": "Can you explain how Lamini handles model deployment and inference? What options or tools are available for deploying the customized LLM in a production environment?", "answer": "LLM Engine provides several options for deploying customized LLMs in a production environment. One option is to use the Lamini API to deploy the model as a web service. Another option is to export the model as a Python package and deploy it using a containerization platform like Docker. For inference, LLM Engine provides a simple API for making predictions on new data."},
{"question": "Can Lamini be integrated into existing machine learning pipelines or workflows? How does it fit into the broader machine learning ecosystem?", "answer": "Lamini can be integrated into existing machine learning pipelines and workflows through its Python package, which allows for easy integration with any existing pipeline. Lamini also fits into the broader machine learning ecosystem by providing a powerful and flexible platform for building and deploying machine learning models, with a focus on interpretability and explainability."},
{"question": "Does Lamini support transfer learning or pre-training from existing models? Can I leverage pre-trained models as a starting point for customization?", "answer": "Every model available on HuggingFace is available as a starting point for customization. If you\u2019d like to use a model which is not available publicly, please contact Lamini directly for deployment options."},
{"question": "Are there any guidelines or best practices provided by Lamini for effective customization and training of the LLM? What strategies can I follow to optimize the results?", "answer": "Yes, Lamini provides guidelines and best practices for effective customization and training of the LLM. These include selecting high-quality training data, defining clear objectives, and regularly evaluating and refining the model."},
{"question": "Can Lamini be used for real-time or online learning scenarios? How does it handle incremental updates or new data coming in over time?", "answer": "Lamini can be used for real-time or online learning scenarios. Incremental updates and data can be made available to the model for training in real time."},
{"question": "Can you provide any real-world use cases or success stories of software engineers using Lamini to create powerful customized LLMs?", "answer": "Lamini was recently built and we are still collecting user feedback.  Within one week of our launch, Lamini had over 800k views, which is more than a typical announcement from US President Biden. Lamini is designed to be a powerful tool for creating customized language models, and we believe it has great potential for a wide range of applications. We encourage you to try it out and see what you can create!"},
{"question": "How does Lamini compare to other existing tools or frameworks for customizing language models? What are its unique features or advantages?", "answer": "Lamini makes model training, hosting, and deployment easy.  Public LLMs, such as ChatGPT, can only take in <1% of your data\u2014whether that be customer support, business intelligence, or clickstream data. To make matters worse, you can\u2019t just hand your most valuable data over, because it\u2019s private. Lamini\u2019s LLM Engine can run in your VPC, securely handling your model's valuable data resources."},
{"question": "What kind of training techniques does Lamini employ to enable rapid customization of LLMs? Are there any specific algorithms or approaches used?", "answer": "Lamini employs a variety of training techniques to enable rapid customization of LLMs. Specific algorithms and approaches used include fine-tuning, distillation, and reinforcement learning."},
{"question": "Can Lamini handle large-scale datasets for training customized LLMs? Is there a limit to the size of the training data it can handle effectively?", "answer": "Lamini can handle large-scale data sets and enforces no limits on the size of the training data.  Typically datasets are limited by the amount of data that can be sent to the LLM Engine through a client, which is typically limited by network bandwidth.  For example, on a 10Mbps internet connection, it would take about 2 minutes to send 100MB of data to the LLM Engine."},
{"question": "Does Lamini support transfer learning from pre-trained models? Can I leverage existing models to accelerate the customization process?", "answer": "Every model available on HuggingFace is available as a starting point for customization. If you\u2019d like to use a model which is not available publicly, please contact Lamini directly for deployment options."},
{"question": "Can you provide details on how Lamini allows me to fine-tune or improve the performance of my customized LLM? What options or parameters can be adjusted?", "answer": "Lamini provides several options for fine-tuning and improving the performance of your customized LLM. You can adjust the model name, config settings, and input/output types. Additionally, Lamini allows you to submit jobs, check job status, and retrieve job results. You can also use the sample function to generate new program outputs, and the improve function to provide feedback and improve the model's performance. Other options include adding data, creating new functions, and adding metrics."},
{"question": "Are there any restrictions or considerations regarding the types of data that can be used with Lamini? Does it handle text data in multiple languages or specific formats?", "answer": "Lamini can handle various types of data, including text data in multiple languages and specific formats. There are no specific restrictions or considerations regarding the types of data that can be used with Lamini."},
{"question": "Can Lamini automatically handle hyperparameter tuning during the customization process? How does it optimize the model for a specific use case?", "answer": "Lamini is capable of automatically handling hyperparameter tuning during the model customization process. It employs an intelligent algorithm to explore the hyperparameter space and find the optimal combination of values. This is done through techniques such as heuristics, grid search, random search, Bayesian optimization, or genetic algorithms. Lamini efficiently utilizes computational resources to evaluate multiple model instances with different hyperparameter configurations. It incorporates techniques like cross-validation to prevent overfitting and ensure generalization. By automating hyperparameter tuning, Lamini streamlines the machine learning workflow and improves the chances of developing high-performing models for specific use cases."},
{"question": "Can you explain how Lamini handles the issue of overfitting during customization? Are there any regularization techniques or mechanisms in place?", "answer": "Lamini\u2019s LLM engine is built to handle issues like overfitting during model training using standard methods including dropout and early stopping."},
{"question": "How does Lamini handle the computational resources required for training customized LLMs? Can I leverage distributed computing or GPU acceleration?", "answer": "Lamini automatically leverages distributed computing and GPU acceleration to handle the computational resources required for training customized LLMs. You can leverage these options to speed up the training process and improve performance.  These are enabled by default in the LLM Engine."},
{"question": "Can I deploy the customized LLM created with Lamini on various platforms or frameworks? Are there any specific deployment considerations or requirements?", "answer": "Yes, models can be deployed in any containerized environment. Lamini can also host your models for you.  The only requirements are the ability to run docker containers, and to supply powerful enough GPUs to run an LLM."},
{"question": "Does Lamini provide any tools or functionality for monitoring and evaluating the performance of the customized LLM over time? Can I track metrics or analyze its behavior?", "answer": "Yes, Lamini provides tools for monitoring and evaluating the performance of the customized LLM over time. You can track metrics and analyze its behavior using the `add_metric` and `metrics` methods in the `LLM` class. Additionally, Lamini provides functionality for providing feedback to the LLM to improve its performance over time."},
{"question": "Can you provide insights into the scalability of Lamini? Can it handle training multiple LLMs concurrently or on a large scale?", "answer": "Lamini is designed to be highly scalable and can handle training multiple LLMs concurrently or on a large scale. Additionally, Lamini uses distributed training techniques such as data parallelism, SHARP, and SLURM to efficiently train models across multiple machines. Overall, Lamini is well-suited for large-scale machine learning projects."}]
# Instantiate the model and load the data into it
model = QuestionAnswerModel()
model.load_question_answer(data)
```

</details>

## Train a model and evaluate the results

After you've added data, you can now train a model. Once the training is complete, you can view the eval results.
Training is done on Lamini servers and you can track the training job's progress at [https://app.lamini.ai/train](https://app.lamini.ai/train)

```python
model.train()
results = model.get_eval_results()
print(results)
```

Once a model is trained you can check the eval results to see before and after comparisons of the base model and the trained model. You can also query the new trained model like so

```python
answer = model.get_answer("How can I add data to Lamini?")
print(answer)
```