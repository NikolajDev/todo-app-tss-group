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

<p align="center">
When creating tasks, you will have a  bolder text as a title and normal text for the "description"

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/625c80c7-3d46-4702-a601-75a3fde91509" />

When the task is complete you will notice the text-dectoration changes, to line-through, that symbolize the 
task is done 

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/823deba3-fb16-487f-8ee6-6ddc4e13f778" />
</p>

# Edit
<p align="center">
You can make mistakes, so you will be able to change anyhting title or description

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/dbb72f20-3b13-4c88-adaa-5481b6a78da6" />

No matter how long the text will be the CSS will take care for a good UI, using the flex display, and the word-breaks automatically when the text line reaches the width of the parents width (p) 
</p>

# Delete
<p align="center">
If you completed the task and you want it to delete just the trash bin.
<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/28541e33-ea02-47ef-811d-2c97806800d9" />

after clicking on the bin
<br>
<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/d17b09da-015c-435b-a7c4-794d53b3b065" />

and if you not sure that you will do this tasks again you can click complete (tick), and you will have an option of making it again like on these 2 pictures
<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/1d354781-7db3-4d71-a548-91269f1f1180" />

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/ac410893-3219-46ae-9e43-f0832db7230d" />
</p>


<p align="center">
also here is the repo that is a learning repo of Laravel by tutorial on their website:
**Tutorial:** [Official Laravel Documentation](https://laravel.com/docs/11.x/installation)  
**Source:** [![GitHub](https://img.shields.io/badge/GitHub-Repo-181717?logo=github&logoColor=white)](https://github.com/NikolajDev/LearningProject)

Thank you for visiting this repository!
</p>
