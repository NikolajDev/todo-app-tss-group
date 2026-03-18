# todo-app-tss-group
Project in laravel with vue.js and database, this project is for company called tss-group

# General

I choose to use and API endpoint, and taking data from it in the json, so it will be easy to work with them,
in the 'v-for="task in tasks"' is best becuase it is similar to working with dictionary in python so Even I did not use the language, it was easy for me to set it up. to get the value we use this dot notation first the object then the properties: task.id will 
Looks like 

```
{
   tasks [
      task1 {
       id: ....,
       name: ...,
       another_key: another_value,
     },
      task2 {
       .: ..,
       .: ..,
     }..
   ]
}
```

I use AI but mainly, but I also look at the code understadning it, how does the REST API works, what are the best practices, what is MVC model, and trying to learn as much as I can. I am also trying to managing the tasks from the University so I do not have so much time, Right now I am focues on learning laravel and vue.js and on side is to keep with my MUNI lectures and tasks from the UNI.

# Create Task

<div align="center">
  <p>When creating tasks, you will have a bolder text as a title and normal text for the "description"</p>
  <img width="700" src="https://github.com/user-attachments/assets/625c80c7-3d46-4702-a601-75a3fde91509" alt="Create Task" />
  
  <p>When the task is complete you will notice the text-decoration changes to line-through, symbolizing the task is done</p>
  <img width="700" src="https://github.com/user-attachments/assets/823deba3-fb16-487f-8ee6-6ddc4e13f778" alt="Complete Task" />
</div>

# Edit

<div align="center">
  <p>You can make mistakes, so you will be able to change anything: title or description</p>
  <img width="700" src="https://github.com/user-attachments/assets/dbb72f20-3b13-4c88-adaa-5481b6a78da6" alt="Edit Task" />
  
  <p>No matter how long the text is, the CSS handles the UI using flex display and automatic word-breaks.</p>
</div>

# Delete

<div align="center">
  <p>If you completed the task and you want to delete it, just use the trash bin.</p>
  <img width="700" src="https://github.com/user-attachments/assets/28541e33-ea02-47ef-811d-2c97806800d9" alt="Delete 1" />

  <p>After clicking on the bin:</p>
  <img width="700" src="https://github.com/user-attachments/assets/d17b09da-015c-435b-a7c4-794d53b3b065" alt="Delete 2" />

  <p>And if you are not sure that you will do these tasks again, you can click complete (tick), and you will have an option of making it again like on these 2 pictures:</p>
  <img width="700" src="https://github.com/user-attachments/assets/1d354781-7db3-4d71-a548-91269f1f1180" alt="Status 1" />
  <br><br>
  <img width="700" src="https://github.com/user-attachments/assets/ac410893-3219-46ae-9e43-f0832db7230d" alt="Status 2" />
</div>

<div align="center">
  <p>Also here is the repo that is a learning repo of Laravel by tutorial on their website:</p>
  <p>
    <b>Tutorial:</b> <a href="https://laravel.com/docs/11.x/installation">Official Laravel Documentation</a><br>
    <b>Source:</b> <a href="https://github.com/NikolajDev/LearningProject"><img src="https://img.shields.io/badge/GitHub-Repo-181717?logo=github&logoColor=white" alt="GitHub Repo"></a>
  </p>
  <br>
  <p><b>Thank you for visiting this repository!</b></p>
</div>
