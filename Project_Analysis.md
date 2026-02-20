# XML Integration Project - Architecture Analysis

## 📋 Architecture Overview

Based on the architecture diagram, here's the complete data flow:

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                              DATA FLOW DIAGRAM                                       │
└─────────────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────┐                                    ┌──────────────────────┐
│  External AI System  │                                    │      EMR App         │
│  (VisceraChart XML)  │                                    │   (User Interface)   │
│  Generates patient   │                                    │  Clinician triggers  │
│  visit data in XML   │                                    │  document generation │
└──────────┬───────────┘                                    └──────────┬───────────┘
           │ Sends XML                                                  │ Data
           ▼                                                            ▼
┌──────────────────────┐                                    ┌──────────────────────┐
│    File Storage      │                                    │       SRS DB         │
│   (FTP/Folder)       │                                    │  (Main EMR Database) │
│  Landing zone for    │                                    │  Patient records,    │
│  incoming XML files  │                                    │  visits, demographics│
└──────────┬───────────┘                                    └──────────┬───────────┘
           │ Files                                                      │
           ▼                                                            │
┌──────────────────────┐       ┌──────────────────────┐                │
│ Job Server(Ingestion)│       │Job Server(Data Link) │                │
│ Watches folder,      │       │ Fetches AI-generated │                │
│ parses XML files,    │       │ data from Staging DB │                │
│ writes to staging DB │       │ for document use     │                │
└──────────┬───────────┘       └──────────┬───────────┘                │
           │ Imports                       │ Fetch                      │
           ▼                               │                            │
┌──────────────────────┐                   │         Populates          │
│ Temporary Staging DB │◄──────────────────┘         document           │
│ (SQL Express/Postgres)                             fields             │
│ Stores AI-generated  │                               │                │
│ data (chief complaint│                               ▼                │
│ narrative, etc.)     │            ┌─────────────────────────────────┐ │
│ Short-lived,         │            │   Microsoft Word Document       │◄┘
│ intermediate         │            │  Template with 2 data sources:  │
└──────────┬───────────┘            │  1) SRS DB for demographics     │
           │                        │  2) Staging DB for AI narrative │
           │                        │  → Final generated document     │
┌──────────┴───────────┐            └─────────────────────────────────┘
│Job Server(Data       │
│Cleanup)              │
│ Removes expired      │
│ visit data from      │
│ Staging DB           │
└──────────────────────┘
```

---

## 🖥️ Servers & Components Mapping

| Component | Server | Status |
|-----------|--------|--------|
| Authentication | UGA-DC-01, UGA-DC-02 | ✅ Available |
| SRS Application | SRS-APP-01 | ✅ Available (you have access) |
| SRS Database | SRS-SQL-01 | ✅ Available |
| Staging DB (SQL Express) | UGA-JOBS-01 | ✅ Installed (VisceraIntegrationDB) |
| Job Server (Ingestion, Data Link, Cleanup) | UGA-JOBS-01 | ⚠️ Needs Implementation |
| External AI System (VisceraChart) | External | ❓ Unknown - Need Client Input |
| File Storage (FTP/Folder) | TBD | ⚠️ Needs Configuration |

---

## 🔧 Tech Stack (Selected)

| Component | Technology | Purpose |
|-----------|------------|---------|
| **Database** | SQL Server Express 2025 | ✅ Already installed on UGA-JOBS-01 (VisceraIntegrationDB) |
| **Job Server / File Watcher** | .NET Windows Service | Watch for XML files, parse data, write to staging DB |
| **Data Processing & Automation** | Python | XML parsing, data transformation, automation scripts |
| **Word Template** | VBA Macros | Data population and template logic |
| **Document Generation** | Office Open XML SDK | Programmatic Word document generation |
| **File Transfer** | Shared folder or FTP | Landing zone for incoming XML files |

---

## ❓ Questions to Ask the Client About AI System

### **Critical Questions - Must Ask:**

1. **AI System Access & Format**
   - "Can you provide a sample XML file that the AI system generates?"
   - "What is the XML schema/structure?"
   - "What fields are included in the XML (e.g., chief complaint, narrative, patient ID, visit ID)?"

2. **Data Delivery Method**
   - "How does the AI system deliver the XML files? (FTP, API, shared folder, email?)"
   - "What is the frequency of XML file generation? (Real-time, hourly, daily?)"
   - "What is the naming convention for XML files?"

3. **Patient/Visit Linking**
   - "How do we link the AI-generated data to existing patient records in SRS?"
   - "What unique identifier connects the AI system data to SRS patients? (Patient ID, MRN, Visit ID?)"

4. **AI System Credentials**
   - "Can I get access credentials to the AI system or its documentation?"
   - "Is there an API available for the AI system?"
   - "Who is the technical contact for the AI system?"

### **Secondary Questions:**

5. **Data Retention**
   - "How long should AI-generated data be retained in the staging database?"
   - "What triggers data cleanup? (Document generation complete, time-based?)"

6. **Word Document Template**
   - "Can you provide the current Word document template being used?"
   - "How is the Word document currently pulling data from SRS?"
   - "What specific fields from the AI system need to appear in the document?"

7. **Error Handling**
   - "What should happen if XML parsing fails?"
   - "Should there be email/alert notifications for errors?"
   - "Is there a logging requirement?"

8. **Timeline & Priority**
   - "What is the expected go-live date?"
   - "Which data fields are highest priority?"

---

## 📋 Implementation Checklist

### Phase 1: Setup & Discovery
- [ ] Get sample XML file from AI system
- [ ] Get access to AI system or documentation
- [ ] Review existing Word document template
- [ ] Understand current EMR-to-Word data flow

### Phase 2: Database Design
- [ ] Design staging table schema based on XML structure
- [ ] Create stored procedures for data operations
- [ ] Test database connectivity from UGA-JOBS-01

### Phase 3: File Monitoring & Ingestion
- [ ] Set up file storage location (FTP/shared folder)
- [ ] Build XML parser component
- [ ] Create file watcher service
- [ ] Test ingestion pipeline

### Phase 4: Word Integration
- [ ] Modify Word template to include second data source
- [ ] Create/update XML mappings in Word
- [ ] Test document generation with both data sources

### Phase 5: Job Server Components
- [ ] Build data link service (fetches data for documents)
- [ ] Build cleanup service (removes expired data)
- [ ] Schedule and test all jobs

### Phase 6: Testing & Go-Live
- [ ] End-to-end testing
- [ ] User acceptance testing
- [ ] Documentation
- [ ] Go-live support

---

## 💻 Connection Details Reference

```
Server: UGA-JOBS-01
Database: VisceraIntegrationDB
Username: visceraintegration
Password: XVQyEmmn4A&!Gh#N
```

---

## 📌 Summary

The architecture is clear and well-designed. The main unknowns are around the **External AI System**. Before implementation can proceed, you need:

1. **Sample XML file** and schema documentation
2. **Access credentials** or technical documentation for the AI system
3. **Understanding of how patient/visit IDs** link between systems
4. **Current Word template** to understand existing data connections

Once you have these, implementation can begin with the database schema design and file ingestion pipeline.
