# Client Call Script - Voice Call Preparation

---

## 📞 Opening

> "Hi Cheris Craig, thank you for making time for this call. I've reviewed the architecture diagram you provided and have a good understanding of what we're trying to accomplish. I've also checked out the SRS application.
>
> I wanted to walk you through my understanding of the workflow and then ask a few questions about the AI system so I can move forward with implementation."

---

## 🔄 Simple Workflow Explanation (Human-Friendly)

> "Let me explain how I understand the system will work, in simple terms:
>
> **Step 1 - AI Creates the Data**  
> Your AI system - the VisceraChart system - will look at patient visits and generate clinical notes, like chief complaints and narratives. This data will be saved as an XML file.
>
> **Step 2 - We Pick Up the File**  
> That XML file lands in a folder - think of it like a dropbox. Our job server on UGA-JOBS-01 is watching that folder. As soon as a new file appears, it picks it up.
>
> **Step 3 - We Store It Temporarily**  
> The job server reads the XML, pulls out the important information, and saves it into our staging database - that's the VisceraIntegrationDB we already set up on UGA-JOBS-01.
>
> **Step 4 - Document Generation**  
> When a clinician opens a patient chart in the EMR app and wants to generate a Word document, here's what happens behind the scenes:
> - The Word template pulls patient demographics and visit info from the SRS database - that's your main EMR database
> - At the same time, it pulls the AI-generated narrative from our staging database
> - These two data sources merge together into one final Word document
>
> **Step 5 - Cleanup**  
> Once we're done, the system cleans up old data from the staging database so it doesn't pile up.
>
> Does that match your understanding of what we're building?"

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

## ✅ Action Items After Call

| Item | Owner | Due Date |
|------|-------|----------|
| Send sample XML file | Client | |
| Provide AI system access/docs | Client | |
| Set up SRS-SQL-01 read access | Client | |
| Share current Word template | Client | |
| Begin database schema design | Me | After receiving XML |
| Build XML parser prototype | Me | After receiving XML |
