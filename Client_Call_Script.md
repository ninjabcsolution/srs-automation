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

> "For technology, here's what we need:
>
> - ✅ **SQL Server database** - Already set up on UGA-JOBS-01
> - 📝 **File watcher & XML parser** - A program to watch for new files and read the data (I'll build this using PowerShell or a Windows service)
> - 📝 **Word template update** - Modify the template to pull data from both databases
> - 📝 **Cleanup scheduler** - A scheduled job to remove old data
> - ❓ **SRS database access** - I need read access to SRS-SQL-01 to query patient info"

---

## 📋 Implementation Steps (Simple)

> "Here's how I'll build this:
>
> 1. **First, I need from you:** Sample XML file, AI system access, SRS database access, and the Word template
> 2. **Then I'll set up the database tables** to store the AI data
> 3. **Build the file watcher** that picks up and processes XML files
> 4. **Update the Word template** to pull from both databases
> 5. **Set up the cleanup job** to remove old data
> 6. **Test everything end-to-end** before going live
>
> Once I have what I need from you, I can start right away."

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

> "One more thing - to complete this integration and generate the final Word documents, I'll need access to query the SRS database on SRS-SQL-01. 
>
> The Word document needs to pull data from two places:
> 1. The **staging database** on UGA-JOBS-01 - I already have credentials for that
> 2. The **main SRS database** on SRS-SQL-01 - I'll need read access to the patient and visit tables
>
> Can you set up a read-only database account for me on SRS-SQL-01? I'll need to be able to query patient demographics, visit information, and any other fields that currently go into the Word document."

---

## 📋 Summary of What I Need from You

> "To summarize, here's what would help me move forward quickly:
>
> 1. **Sample XML file** from the AI system
> 2. **Information on how XML files are delivered** (folder path, FTP credentials, or API details)
> 3. **The unique identifier** that links AI data to SRS patients
> 4. **Access to AI system** or its documentation
> 5. **Read access to SRS-SQL-01** database for patient/visit data
> 6. **Current Word template** if you have it handy
>
> Once I have these, I can start building and testing right away."

---

## 📞 Closing

> "Do you have any questions for me about the approach, the technology, or the implementation steps? Is there anything you'd like to change or that I might have misunderstood?
>
> Great - I'll wait for those items from you, and as soon as I receive them, I'll get started. Feel free to email them to me or share them however works best for you.
>
> Thanks again for your time!"

---

## 📝 Notes Section (Fill During Call)

**AI System Delivery Method:**  
_________________________________

**Unique Identifier for Patient Linking:**  
_________________________________

**AI System Contact Person:**  
_________________________________

**Expected Timeline for Getting Sample XML:**  
_________________________________

**SRS Database Access - Who to Contact:**  
_________________________________

**Other Notes:**  
_________________________________
_________________________________
_________________________________

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
