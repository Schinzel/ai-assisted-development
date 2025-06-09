## **Purpose**

The purpose of this page is to provide a workflow for how to work effectively with AI to implement a story, task or project.
Make sure that the project description and the code standards are in place and up to date.

## **1 Write Instructions**
* Create a description of the task that the AI is to solve.   
* Suggestions to instructions  
  * Read this [guide](https://github.com/Qais-Hweidi/ai-assisted-development-guide/blob/main/lessons/01-project-structure.md)  
  * Create small clearly defined tasks
  * If appropriate, divide into phases.
  
The reason you want them in a markdown file is so that:
1) an AI assistant can improve the document
2) an AI agent can easily read and add to the document. 

I have created a template for tasks here [template_tasks_phase_N.md](./template_tasks_phase_N.md).
Link to the task files in [task_overview.md](./task_overview.md)

## **2 Improve instructions**
Use an AI assistant such as Claude desktop to refine the instructions for an AI agent.

### **Suggestion of prompt**

<em>You are an experienced architect with many years of development experience.

In the directory /Users/my_name/code/my_project:
- Read the file README.md and follow all its links so that you understand the project, stack, and code standards.
- Read the file doc/tasks/task_overview.md and follow all its links so that you understand completed and upcoming tasks.

Your job is to in collaboration with me create and modify the tasks so that an AI agent can successfully implement the tasks.

Tell me when you are done reading</em>

## **3 Code changes**

Let an AI agent such as Claude Code do the actual implementation. Start a new session for each task.

### **Suggestion of prompt**

<em>You are an experienced architect with many years of development experience.

In the directory /Users/my_name/code/my_project:
- Read the file README.md and follow all its links so that you understand the project, stack, and code standards.
- Read the file doc/tasks/task_overview.md and follow all its links instructions so that you understand completed and upcoming tasks.

Do the next task that is to be done. Do only this task and nothing else. Tell me when you are done.</em>

Restart the agent between tasks to prevent excessive token usage and to keep clear context.

