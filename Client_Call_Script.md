# Client Call Script - Voice Call Preparation

---

## 📞 Opening

> "Hi Cheris Craig, thank you for making time for this call. I've reviewed the architecture diagram you provided and have a good understanding of what we're trying to accomplish. I've also checked out the SRS application.
>
> I wanted to walk you through my understanding of the workflow, explain what technology we'll need, and give you a sense of the implementation steps. Then I have a few questions about the AI system so I can move forward."

---

## 🔄 How The System Will Work (Simple Explanation)

> "Let me walk you through how this whole thing works, step by step, in plain English:
>
> **Imagine it like a relay race with the data being passed from one runner to the next.**
>
> ---
>
> **First, the AI does its job.**  
> The VisceraChart AI system listens to patient visits and writes up clinical notes - things like what the patient came in for, the chief complaint, and a narrative summary. Once it's done, it saves all of that into an XML file. Think of XML as just a structured text file that computers can easily read.
>
> ---
>
> **Next, we catch that file.**  
> That XML file gets dropped into a folder - like putting a letter in a mailbox. On our end, we have a little program running on the UGA-JOBS-01 server that's constantly watching that folder. The moment a new file shows up, it grabs it.
>
> ---
>
> **Then, we read and store the data.**  
> Our program opens up that XML file, reads through it, and pulls out all the important pieces - the patient ID, the narrative, the chief complaint, whatever's in there. Then it saves that information into a temporary database. We already have this database set up - it's called VisceraIntegrationDB on the UGA-JOBS-01 server.
>
> ---
>
> **Now comes the magic - generating the Word document.**  
> When a clinician is in the EMR app and opens a patient's chart, they can generate a Word document. Here's what happens behind the scenes:
>
> - The Word template reaches out to TWO databases
> - First, it grabs patient demographics and visit information from your main SRS database - things like name, date of birth, address, visit date
> - Second, it grabs the AI-generated narrative from our staging database - the clinical notes the AI wrote
> - The template merges both together and creates one complete document
>
> So the final document has BOTH the structured EMR data AND the AI-written narrative, all in one place.
>
> ---
>
> **Finally, we clean up.**  
> After the document is generated and we don't need the AI data anymore, a cleanup job removes the old records from the staging database. This keeps things tidy and makes sure we're not storing patient data longer than necessary.
>
> ---
>
> **Does that make sense? Is that how you envisioned it working?**"

---

## 🛠️ What Technology We Need (In Plain English)

> "Now let me explain what technology pieces we need to make this work. Don't worry, I'll keep it simple:
>
> ---
>
> **1. A Database to Store the AI Data**  
> ✅ Good news - this is already done! We have SQL Server Express installed on UGA-JOBS-01, and the database called VisceraIntegrationDB is already created. This is where we'll temporarily store the AI-generated content until it gets used.
>
> ---
>
> **2. A File Watcher Program**  
> We need a small program that runs in the background and watches a folder for new XML files. When a file appears, it processes it automatically. I can build this using:
> - PowerShell scripts (simple and works great on Windows servers)
> - Or a small Windows service application
>
> This will run on UGA-JOBS-01 since that's where the database is.
>
> ---
>
> **3. An XML Parser**  
> This is the piece that reads the XML file and extracts the data. It's part of the file watcher - once we grab the file, we need to understand what's inside. I can build this once I see a sample XML file from the AI system.
>
> ---
>
> **4. Database Connection to SRS**  
> To generate the final Word document, we need to read from TWO databases:
> - The staging database on UGA-JOBS-01 (already have access)
> - The main SRS database on SRS-SQL-01 (need access to this one)
>
> So I'll need read access to the SRS database to query patient and visit information.
>
> ---
>
> **5. Word Document Template with Two Data Sources**  
> The Word template needs to be set up to pull data from two places. This might involve:
> - Custom XML parts in Word
> - Or some VBA macros
> - Or modifying how the template connects to databases
>
> I'll need to see the current template to understand how it works today, then I can add the second data source.
>
> ---
>
> **6. A Cleanup Scheduler**  
> A simple scheduled task that runs periodically (maybe once a day or once a week) and removes old records from the staging database. This is straightforward - just a scheduled job on the server.
>
> ---
>
> **In summary, the main technology pieces are:**
> - ✅ SQL Server Express database (already set up)
> - 📝 File watcher / XML parser (I'll build this)
> - 📝 Word template modification (need to see current template)
> - 📝 Cleanup scheduler (simple to set up)
> - ❓ Access to SRS database (need from you)
>
> **Does this technology list make sense to you?**"

---

## 📋 Implementation Steps (What We'll Do and When)

> "Let me walk you through how I plan to build this, step by step:
>
> ---
>
> **Phase 1: Getting Everything I Need** *(This is where you come in)*
>
> Before I can start building, I need a few things from you:
> - A sample XML file from the AI system so I can see the structure
> - Access to the AI system or documentation about it
> - Database access to SRS-SQL-01 so I can query patient data
> - The current Word template you're using
>
> Once I have these, I can start building right away.
>
> ---
>
> **Phase 2: Setting Up the Database Tables**
>
> First thing I'll do is design the database tables in our staging database. These tables will hold the AI-generated data - things like:
> - Patient identifier (so we can match it to SRS)
> - Chief complaint
> - Narrative text
> - Timestamps for when the data came in
>
> This is quick work - probably a day or two once I see the XML structure.
>
> ---
>
> **Phase 3: Building the File Watcher and Parser**
>
> Next, I'll create the program that:
> - Watches the folder where XML files land
> - Picks up new files automatically
> - Reads the XML and extracts the data
> - Saves it to the staging database
> - Moves or archives the processed file
>
> I'll test this thoroughly with sample files to make sure it handles everything correctly.
>
> ---
>
> **Phase 4: Connecting the Word Template**
>
> This is the integration part. I'll:
> - Look at your current Word template and understand how it works
> - Add a second data connection to our staging database
> - Map the AI-generated fields to places in the document
> - Test to make sure both data sources populate correctly
>
> ---
>
> **Phase 5: Setting Up the Cleanup Job**
>
> I'll create a scheduled task that:
> - Runs on a schedule (daily, weekly - whatever makes sense)
> - Removes old records from the staging database
> - Maybe keeps a log of what was cleaned up
>
> ---
>
> **Phase 6: Testing Everything End-to-End**
>
> Finally, we'll test the whole flow:
> - Drop a test XML file in the folder
> - Watch it get processed
> - Open a patient chart in the EMR
> - Generate a Word document
> - Verify all the data shows up correctly
>
> Once we're confident it works, we can go live.
>
> ---
>
> **How does that timeline sound? Is there anything you'd want me to prioritize?**"

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
