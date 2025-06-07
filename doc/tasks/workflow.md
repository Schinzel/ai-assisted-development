## **Purpose**

The purpose of this page is to provide a guide for how to work effectively with AI to implement a story.

## **1 Write Instructions**

* Create a description of the task that the AI is to solve.   
* Suggestions to instructions  
  * Read this [guide](https://github.com/Qais-Hweidi/ai-assisted-development-guide/blob/main/lessons/01-project-structure.md)  
  * Create small clearly defined tasks  
* Add the instructions to the project as a markdown file, for example TASK.md. 
You can write the instructions in Google Doc and when you are happy with them export to markdown.
The reason you want them in a markdown file is so that 1\) an AI assistant can improve the document and 2\) an AI agent can easily read and add to the document. 
I have created a template for [TASK.md](./template_TASK.md). Feel free to improve on this template file.

## **2 Improve instructions**

Use an AI assistant such as Claude desktop to refine the instructions for an AI agent.

### **Suggestion of prompt**

"*Read the file /Users/my_name/code/my_project/README.md and follow all its links to understand the project, the stack and the code standards.*

*In the file /Users/my_name/code/my_project/TASK.md are the instructions for an AI agent.*

*You are to help me improve the instructions for an AI agent that will do the actual code changes. The purpose of this chat session is to improve the instructions for an AI.*

* *You are not to change any code in the project.*  
* *Do not add code to the document.*   
* *Suggest one change in the document at a time.*   
* *For each suggestion, explain the suggestion and ask if you should do any changes in TASK.md*"

## **3 Code changes**

Let an AI agent such as Claude Code do the actual implementation. Start a new session for each task.

### **Suggestion of prompt**

*"Read the file /Users/my_name/code/my_project/README.md and follow all its links to understand the project, the stack and the code standards.* 

*In the file /Users/my_name/code/my_project/TASK.md are a series of tasks that I want you to implement and instructions on how to implement them."*

## **4 Finishing up**

When you are all done, remove the file TASK.md and create the pull request.  
