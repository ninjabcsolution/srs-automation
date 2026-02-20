# Client Call Script - Voice Call Preparation

---

## 📞 Opening

> "Hi Cheris Craig, thank you for making time for this call. I've reviewed the architecture diagram you provided and have a good understanding of what we're trying to accomplish. I've also checked out the SRS application.
>
> I wanted to walk you through my understanding of the workflow, explain what technology we'll need, and give you a sense of the implementation steps. Then I have a few questions about the AI system so I can move forward."

---

## 🔄 Workflow (Simple)

> "Here's how it works in a nutshell:
>
> The AI system generates clinical notes and saves them as XML files. Our job server picks up those files, reads the data, and stores it in a staging database. When a clinician generates a Word document, the template pulls patient info from the SRS database AND the AI narrative from our staging database - merging them into one final document. Then we clean up old data automatically.
>
> That's it - AI creates data, we catch it, store it, and merge it with your EMR data into Word documents."

---

## 🛠️ Technology We Need (Simple)

> "The database is already set up. Here's the tech stack I'll use:
>
> - **.NET Windows Service** - to watch for XML files and process them
> - **Python** - for data processing and automation
> - **VBA Macros** - in the Word template for data population
> - **Office Open XML SDK** - for programmatic document generation
>
> The only thing I need from you is read access to the SRS database."

---

## 📋 Implementation Steps (Simple)

> "Once I get the sample XML and database access from you, I'll design the database tables, build the file watcher and parser, update the Word template, and set up the cleanup job. Then we'll test everything end-to-end before going live. It's straightforward once I have the pieces."

---

## ❓ Questions to Ask About AI System

> "Now, to move forward, I need some information about the AI system - the VisceraChart. I don't have access to it yet, so I have a few questions:"

### Question 1: Sample XML File
> "Can you send me a sample XML file that the AI system generates? I need to see the actual structure so I can build the parser correctly. Even a test file or dummy data would work."

### Question 2: XML Delivery Method
> "How will the AI system deliver these XML files to us? Will it drop them into a shared folder? Send via FTP? Or is there an API we can call? I need to know so I can set up the right kind of listener."

### Question 3: Linking to Patient Records
> "This is important - how do we connect the AI data to the right patient in SRS? Is there a common identifier like a Patient ID, Medical Record Number, or Visit ID that appears in both the XML and the SRS database? I need this to make sure the right narrative goes to the right patient."

### Question 4: AI System Access
> "Can I get access to the AI system itself, or at least the documentation? It would help me understand the data format better and test the connection. If there's a technical contact at the AI vendor, that would be helpful too."

---

## 🔐 What I Need - SQL Server Access

> "I already have access to the staging database on UGA-JOBS-01. But to generate the final Word documents, I'll also need read access to the SRS database on SRS-SQL-01 to query patient demographics and visit info. Can you set that up for me?"

---

## 📋 Summary of What I Need from You

> "To summarize, I need: a sample XML file from the AI system, information on how the XML files get delivered, access to the AI system or its documentation, read access to the SRS database, and the current Word template. Once I have these, I can start right away."

---


## ✅ Action Items After Call

| Item | Owner | Due Date |
|------|-------|----------|
| Send sample XML file | Client | |
| Provide AI system access/docs | Client | |
| Set up SRS-SQL-01 read access | Client | |
| Share current Word template | Client | |
| Begin database schema design | Me | After receiving XML |
| Build XML parser prototype | Me | After receiving XML |
