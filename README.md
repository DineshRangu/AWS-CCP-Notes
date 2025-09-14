# AWS-CCP-Notes
Handy AWS CCP notes covering EC2, S3, IAM, and key cloud concepts, packed with tips and explanations to make learning AWS simple and exam prep stress-free.

## What’s an EBS Volume?
• An EBS (Elastic Block Store) Volume is a network drive you can attach
to your instances while they run
• It allows your instances to persist data, even after their termination
• They can only be mounted to one instance at a time (at the CCP
level)
• They are bound to a specific availability zone
• Analogy: Think of them as a “network USB stick”
• Free tier: 30 GB of free EBS storage of type General Purpose (SSD) or
Magnetic per month
EBS Volume
• It’s a network drive (i.e. not a physical drive)
• It uses the network to communicate the instance, which means there might be a bit of
latency
• It can be detached from an EC2 instance and attached to another one quickly
• It’s locked to an Availability Zone (AZ)
• An EBS Volume in us-east-1a cannot be attached to us-east-1b
• To move a volume across, you first need to snapshot it
• Have a provisioned capacity (size in GBs, and IOPS)
• You get billed for all the provisioned capacity
• You can increase the capacity of the drive over time
AWS S3-SECURITY POLICILIES

1. **🔒 Private access by default**  
    → All new S3 buckets and objects are **not public unless you allow it**.
    
2. **📜 Bucket policies**  
    → Rules that control **who can access** your S3 bucket and what they can do.
    
3. **🔗 Presigned URLs**  
    → Temporary, secure links to **give limited-time access** to a file.
    
4. **📍 Amazon S3 access points**  
    → Custom access settings for **specific users or applications**.
    
5. **📝 Amazon S3 audit logs**  
    → Logs that show **who accessed what and when**, for tracking and security.


**AWS S3 LIFECYCLE**

 📦 **Example Lifecycle Rule:**

- 🔹 Store in `S3 Standard` for 30 days
    
- 🔸 Move to `Standard-IA` after 30 days
    
- 🔸 Move to `Glacier Deep Archive` after 90 days
    
- 🔴 Delete after 1 year
  
  ### 📦 **S3 Storage Classes – Summary Table**

| **Storage Class**                 | **Definition (Use Case)**                         | **Advantages ✅**                         | **Disadvantages ❌**                     |
| --------------------------------- | ------------------------------------------------- | ---------------------------------------- | --------------------------------------- |
| **S3 Standard**                   | For frequently accessed data                      | High speed, durability, multi-AZ         | Most expensive                          |
| **S3 Standard-IA**                | For infrequent access, but quick retrieval        | Lower cost than Standard, still multi-AZ | Retrieval cost applies                  |
| **S3 One Zone-IA**                | Infrequent access, **stored in 1 AZ** only        | Cheaper than Standard-IA                 | Less durability (risk if AZ fails)      |
| **S3 Glacier Instant Retrieval**  | Archived data that still needs **instant access** | Lower cost than Standard-IA              | Higher cost than deeper Glacier classes |
| **S3 Glacier Flexible Retrieval** | Archive data with **minutes-to-hours** retrieval  | Much cheaper than instant classes        | Not suitable for real-time access       |
| **S3 Glacier Deep Archive**       | For data rarely accessed (e.g., backups)          | **Lowest storage cost**                  | **Hours** to retrieve data              |
| **S3 Intelligent Tiering**        | Automatically moves data based on usage           | Optimized cost, no manual management     | Slight monthly monitoring fee           |

---

### 💡 Example Use Cases:
- **S3 Standard** → Website images, frequently accessed docs
    
- **S3 Standard-IA** → Monthly reports
    
- **One Zone-IA** → Re-creatable data or backups
    
- **Glacier** → Legal records, compliance data
    
- **Intelligent Tiering** → Mixed or unpredictable access patterns


AWS DATABASES
Databases Intro
• Storing data on disk (EFS, EBS, EC2 Instance Store, S3) can have its limits
• Sometimes, you want to store data in a database…
• You can structure the data
• You build indexes to efficiently query / search through the data
• You define relationships between your datasets
• Databases are optimized for a purpose and come with different
features, shapes and constraints

Relational Databases:
Establishing relations int he data in a table

NoSQL Databases
• NoSQL = non-SQL = non relational databases
• NoSQL databases are purpose built for specific data models and have
flexible schemas for building modern applications.

### **Benefits of NoSQL Databases (Simplified)**

1. **Flexibility**  
    → Easy to change or evolve the data model over time.
    
2. **Scalability**  
    → Can scale out easily using distributed clusters (adds more servers instead of upgrading one).
    
3. **High Performance**  
    → Optimized for fast access based on a specific data model (e.g., key-value, document).
    
4. **Highly Functional**  
    → Supports different data types and models optimized for specific use cases.
    
 📚 **Databases & Shared Responsibility – Simplified**

- ✅ **AWS helps manage databases** (like RDS, DynamoDB).
    
- ✅ **Benefits**:
    
    - Fast setup (Quick provisioning)
        
    - Built-in high availability & scaling
        
    - Automatic backups and updates
        
    - AWS handles OS patching
        
    - Monitoring and alerts provided
        
- ⚠️ **Note**:  
    If you run a database on **EC2**, **you must manage everything** yourself:  
    ➝ backups, updates, scaling, patching, and more.
    
    ### Amazon Aurora

Amazon **Aurora** is a special type of **database service** made by AWS. It’s like a **faster, smarter version** of MySQL or PostgreSQL, built to run better on the AWS cloud.

---

### 🔹 Key Points (Simplified):

1. **Proprietary**  
    Aurora is owned by AWS. It’s not open-source like MySQL or PostgreSQL.
    
2. **Compatible**  
    You can use Aurora as if it were **MySQL or PostgreSQL**, so no need to change your code.
    
3. **Faster**  
    Aurora is:
    
    - **5x faster than MySQL on RDS**
        
    - **3x faster than PostgreSQL on RDS**  
        _(RDS = another database service by AWS)_
        
4. **Auto-scaling storage**  
    Storage automatically grows by **10GB chunks**, up to **128 TB** – no manual intervention needed.
    
5. **More expensive**  
    It costs about **20% more than normal RDS**, but gives better speed and reliability.
    

---

### ✅ Real-life Example:

If your website uses MySQL and suddenly gets millions of users (like during a sale), **Aurora can handle the traffic better** than regular MySQL on RDS – it’s faster and scales automatically.

## Amazon Aurora Serverless

**Aurora Serverless** is a version of Amazon Aurora that **automatically starts, stops, and scales** the database based on how much you're using it.  
👉 Think of it like a **“smart” database** that runs only when needed – and pauses when idle.

---

## 🔹 Key Features (Simplified):

|Feature|Explanation|
|---|---|
|✅ **Auto-scaling**|Grows or shrinks automatically based on load. No manual scaling required.|
|🧠 **No capacity planning**|You don’t have to guess how much CPU or memory you’ll need.|
|💸 **Pay per second**|You’re charged **only when the DB is running**. Saves money on light use.|
|🧘 **Minimal management**|AWS handles everything behind the scenes – very low maintenance.|
|🌐 **Supports MySQL & PostgreSQL**|Works just like regular Aurora, so your app doesn’t need major changes.|
|📈 **Best for variable workloads**|Ideal for apps that aren’t always active (e.g., event-based apps, testing)|

---

## 🔹 Key Components (Simplified Terms):

- **Client**: Your application trying to connect to the database.
    
- **Proxy Fleet** (Managed by Aurora): Handles connections smartly and routes them to the database.
    
- **Shared Storage Volume**: A centralized place where your data is stored – safe, fast, and scalable.
    

---

## 🔸 Real-Life Example:

Imagine a **college event registration portal** that gets used heavily only during events.  
Using **Aurora Serverless**, the database:

- Automatically **wakes up** during the event.
    
- **Handles traffic smoothly** with scaling.
    
- **Goes to sleep** after the event, saving cost.
  
  ## Amazon ElastiCache Overview

Just like **RDS gives you managed relational databases**,  
**ElastiCache gives you a managed in-memory cache** (Redis or Memcached) — fully handled by AWS.

---

## 🔹 Key Points (Simplified):

|Feature|Explanation|
|---|---|
|🚀 **In-memory cache**|Data is stored in memory (RAM), so it’s **very fast** — much faster than disk.|
|📚 **Supports Redis & Memcached**|You can choose either Redis (popular) or Memcached (simpler).|
|📉 **Reduces database load**|Great for **read-heavy apps** (e.g., frequent product page views).|
|🔧 **Managed by AWS**|AWS handles all the technical stuff:  <br>OS updates, backups, failovers, monitoring, setup.|
|⚡ **Low latency**|Responses in **milliseconds or less** — ideal for real-time use cases.|

---

## 🔹 Real-Life Example:

Imagine a **shopping website** where thousands of users keep checking the **same product price**.

- Instead of hitting the main database every time,
    
- The app checks **ElastiCache**, which stores the price in memory.
    
- It's **much faster** and **reduces stress** on your database.
    

---

### ✅ Use Cases:

- Leaderboards in gaming
    
- Session stores (e.g., login tokens)
    
- Frequently accessed user data (profile, cart)
    
- Real-time analytic
  
  ## **DynamoDB Accelerator (DAX)**:

---

## 🔹 What is DAX?

-**DAX (DynamoDB Accelerator)** is a **fully managed in-memory cache** **just for DynamoDB**.  
-It reduces read time 
-It’s like giving **super speed** to your DynamoDB queries — reducing response time from milliseconds to **microseconds**.

---

## 🔹 Key Features (Simplified):

| Feature                         | Explanation                                                               |
| ------------------------------- | ------------------------------------------------------------------------- |
| ⚡ **10x faster performance**    | Speeds up DynamoDB by caching data in memory — from **ms to µs latency**. |
| 🔄 **Integrated with DynamoDB** | Works only with DynamoDB, no changes needed in your application logic.    |
| 🔒 **Secure & Managed**         | AWS handles security, scaling, patching, monitoring — you manage nothing. |
| 🌐 **Highly Available**         | DAX automatically replicates across nodes for **fault-tolerance**.        |

---

## 🔹 DAX vs ElastiCache (at AWS Cloud Practitioner - CCP Level):

|Feature|DAX|ElastiCache|
|---|---|---|
|🛠️ **Purpose**|For DynamoDB only|Works with any app or DB|
|⚙️ **Integration**|Built-in with DynamoDB|Requires manual integration|
|⚡ **Latency**|Microseconds (µs)|Milliseconds (ms)|
|🧠 **Use Case**|High-speed read/write from DynamoDB|General caching (web, sessions, etc.)|

---

## 🔸 Real-Life Example:

Let’s say you're building a **mobile app for train ticket booking** using DynamoDB.  
If thousands of users are checking seat availability at the same time, **DAX caches the data**, so users get near-instant results — without overloading the database.

## **DynamoDB Global Tables?

**DynamoDB Global Tables** let you **replicate your database across multiple AWS Regions**,  
so users around the world can **access data quickly and reliably** — no matter where they are.

---

### 🔑 Key Features (Simplified):

|Feature|Explanation|
|---|---|
|🌍 **Multi-region access**|The same DynamoDB table is available in multiple AWS Regions.|
|⚡ **Low latency**|Users near any region get **fast access** to data.|
|🔄 **Active-Active replication**|You can **read & write** in **any region** — changes are synced globally.|
|🔐 **Fault tolerance**|If one region fails, others keep working. Great for **disaster recovery**.|

---

### 🔸 Real-Life Example:

Imagine you run a **travel booking app** with users in the **US, Europe, and Asia**.  
If you store data only in the US region, users in Asia face delays.

With **DynamoDB Global Tables**:

- A user in Asia writes booking data in the **Asia region**.
    
- The same data is auto-replicated to the **US and Europe**.
    
- Everyone sees consistent, up-to-date info — **super fast**.

  
## **Amazon Redshift** 

**Amazon Redshift** is a **cloud data warehouse** service by AWS.  
It’s used for **analyzing large amounts of data** (not for handling frequent app transactions).  
Think of it as a **super-fast database built for reports and analytics**, not day-to-day operations.

---

### 🔑 Key Features (Simplified):

|Feature|Explanation|
|---|---|
|📊 **OLAP (not OLTP)**|Designed for **analytics**, not for storing/updating app data in real-time.|
|📥 **Hourly data loads**|Data is typically **loaded in batches**, not every second.|
|⚡ **High performance**|Runs **10x faster** than traditional data warehouses.|
|🧱 **Columnar storage**|Stores data in **columns** instead of rows, making big queries much faster.|
|🧠 **MPP – Massively Parallel Processing**|Splits tasks across multiple nodes for **faster execution**.|
|🌍 **Highly available & scalable**|Scales to **petabytes** of data without slowing down.|
|💸 **Pay as you go**|Pay based on the size/type of **instances you use**.|
|📝 **SQL interface**|Use **regular SQL** to write queries.|
|📈 **BI tool integration**|Works with tools like **Tableau** and **Amazon QuickSight** for dashboards.|

---

### 🔸 Real-Life Example:

Imagine a company that collects **millions of sales records** every day across the world.  
They want to:

- See **monthly sales trends**
    
- Analyze **which products sell best**
    
- Visualize reports using **Tableau**
    

Instead of using a normal database, they use **Redshift** to:

- Store all that data in columns
    
- Run fast analytical queries
    
- Connect with BI tools to make dashboards
  
  ## Redshift Serverless

**Redshift Serverless** lets you **run data analytics** without setting up or managing any servers.  
It automatically provides the power you need, runs your queries, and then scales down —  
so you **only pay for what you use**.

---

### 🔑 Key Features (Simplified):

|Feature|Explanation|
|---|---|
|⚙️ **No setup needed**|No need to choose or manage instances — AWS handles everything.|
|📈 **Auto-scaling**|Grows and shrinks automatically based on your workload.|
|💸 **Pay-per-use**|Charged only while your queries run — ideal for **cost-saving**.|
|📊 **Use cases**|Great for **dashboards**, **reports**, and **real-time analytics**.|
|💻 **Same SQL interface**|Works like regular Redshift — supports SQL and BI tool integrations.|

---

### 🔸 Real-Life Example:

Suppose your company creates **sales reports every morning**, but does no analysis the rest of the day.

With **Redshift Serverless**:

- AWS automatically **starts and runs** your data warehouse during reporting time.
    
- It **scales up** to run reports fast, then **scales down** after.
    
- You’re billed only for the **time and compute used**, not 24/7.
  
  ## Amazon EMR

**Amazon EMR** is a service that helps you **quickly set up big data clusters** (like Hadoop and Spark) on AWS.  
It’s designed to **analyze and process huge amounts of data** using powerful open-source tools.

---

### 🔑 Key Features (Simplified):

|Feature|Explanation|
|---|---|
|🏗️ **Cluster creation**|Creates **Hadoop/Spark clusters** easily with **hundreds of EC2 instances**.|
|🛠️ **Fully managed**|AWS handles **setup, provisioning, and configuration**.|
|🔥 **Supports multiple frameworks**|Works with **Apache Spark, HBase, Flink, Presto**, and more.|
|📏 **Auto-scaling support**|Can scale **up/down automatically** based on load.|
|💸 **Spot instance integration**|Uses **cheaper EC2 spot instances** to reduce cost.|
|📊 **Use cases**|Ideal for **big data processing**, **machine learning**, **log analysis**, **web crawling**, etc.|

---

### 🔸 Real-Life Example:

Suppose you're a company analyzing **millions of customer reviews** to detect trends.

- You can launch an **EMR cluster** with Spark.
    
- Process all your data in **parallel** across many EC2 instances.
    
- When done, the cluster shuts down — you only pay for the time used.
  
  
  ## Amazon Athena

**Amazon Athena** is a **serverless query tool** that lets you **analyze data directly stored in Amazon S3** — using just **SQL**.  
No servers to set up, no databases to manage. Just point it at your data and start querying.

---

### 🔑 Key Features (Simplified):

|Feature|Explanation|
|---|---|
|🧠 **Serverless**|No need to set up or manage infrastructure — Athena runs automatically.|
|📝 **Uses SQL**|You write **normal SQL queries** to get insights from your data.|
|📁 **Reads from S3**|Data stays in S3 — Athena reads and analyzes it **directly**.|
|📦 **Supports many formats**|Works with **CSV, JSON, Parquet, ORC, Avro**, etc.|
|💸 **Pricing**|**$5 per TB scanned** — so use **compressed or column-based** formats to save cost.|
|📊 **Use cases**|BI reports, log analysis (VPC Flow Logs, CloudTrail, ELB Logs), ad hoc queries.|

---

### 🔸 Real-Life Example:

Let’s say your application stores **website logs** in S3 every hour.  
You want to:

- Find out which IPs hit your site the most.
    
- Track error responses over time.
    

With **Athena**, you:

- Write a **simple SQL query**.
    
- It scans the log files in S3.
    
- Returns the result **within seconds** — no infrastructure needed.
  
  ## Amazon QuickSight?

**Amazon QuickSight** is a **serverless business intelligence (BI) tool** that helps you **create interactive dashboards and visualizations** from your data.  
It even uses **machine learning** to give you smart insights — and it **scales automatically**.

---

### 🔑 Key Features (Simplified):

|Feature|Explanation|
|---|---|
|📊 **Interactive dashboards**|Easily build charts, graphs, and reports from your data.|
|🤖 **ML-powered insights**|Automatically detects trends, anomalies, and forecasts.|
|⚙️ **Serverless & scalable**|No setup needed — it scales to thousands of users automatically.|
|💸 **Per-session pricing**|You **only pay** when users interact with dashboards — cost-effective.|
|🔗 **Data source integration**|Connects with **Athena, Redshift, RDS, Aurora, S3**, and more.|
|📎 **Embeddable**|You can **embed dashboards** inside apps or websites.|

---

### 🔸 Use Cases:

- **Business analytics**: Track KPIs, sales, and performance.
    
- **Ad-hoc analysis**: Run quick, one-time queries or visualizations.
    
- **Dashboard creation**: Show real-time data trends for teams or customers.
    
- **Data storytelling**: Convert raw data into visual insights for decision-making.
    

---

### 🔸 Real-Life Example:

A retail company wants to **track sales across regions** and **see which products perform best**.

With **QuickSight**, they can:

- Pull data from **Amazon Redshift or S3**
    
- Build **interactive charts and dashboards**
    
- Share these with managers — who get insights in seconds without needing SQL.
  
  
  ## Amazon DocumentDB

**Amazon DocumentDB** is a **fully managed NoSQL database** service by AWS that's designed to work like **MongoDB**(Non schema oriendted data.) 
It's ideal for storing and querying **JSON-like documents**, and is built for **high performance at scale**.

---

### 🔑 Key Features (Simplified):

| Feature                             | Explanation                                                                 |     |
| ----------------------------------- | --------------------------------------------------------------------------- | --- |
| 🧾 **MongoDB-compatible**           | Works like MongoDB — you can store and query **JSON data**.                 |     |
| ⚙️ **Fully managed by AWS**         | AWS handles setup, backups, scaling, patching, and availability.            |     |
| 🔁 **High availability**            | Replicates data across **3 Availability Zones** for durability.             |     |
| 📈 **Auto-scaling storage**         | Storage **grows automatically** in 10GB chunks — no manual planning needed. |     |
| ⚡ **Handles large workloads**       | Can scale to **millions of requests per second**.                           |     |
| 🧠 **Deployment model like Aurora** | Similar in architecture: compute and storage are **decoupled**.             |     |
|                                     |                                                                             |     |
### Real-Life Example:

Let’s say you're building a **real-time ride-sharing app**. You need to:

- Store dynamic data like user profiles, ride details, and locations (all in JSON format).
    
- Handle **millions of reads/writes per second**.
    

With **DocumentDB**, you get:

- MongoDB-like flexibility
    
- AWS-level reliability and scaling
    
- No infrastructure to manage

## Amazon Timestream

**Amazon Timestream** is a **serverless, fully managed database** made for storing and analyzing **time series data** — data that changes over time, like temperature readings, app activity, or sensor logs.

---

### 🔑 Key Features (Simplified):

|Feature|Explanation|
|---|---|
|🕒 **Time series database**|Best for data with timestamps (e.g., CPU usage every second).|
|⚙️ **Fully managed & serverless**|No setup or servers — AWS handles everything.|
|📈 **Auto-scaling**|Grows or shrinks automatically as your data increases or decreases.|
|🚀 **Super fast & low cost**|**1000x faster** and **10x cheaper** than using regular relational databases.|
|🧠 **Built-in analytics**|Helps you find patterns and trends in **near real-time**.|
|📊 **Use case**|IoT apps, dashboards, app monitoring, time-based reports.|

---

### 🔸 Real-Life Example:

Say you're monitoring **room temperature sensors** in 1000 buildings:

- Every second, new data is generated.
    
- You want to **store, analyze, and visualize** this data quickly.
    
- With **Timestream**, it’s easy, fast, and cheap — and you don’t have to manage any servers.
  
## Amazon QLDB

**Amazon QLDB** is a **fully managed, serverless database** designed to store a **complete and unchangeable history** of your application data — like a **digital ledger** (similar to how banks track transactions).

---

### 🔑 Key Features (Simplified):

|Feature|Explanation|
|---|---|
|📘 **Ledger database**|Keeps a **permanent history** of all changes — like an accounting ledger.|
|🔒 **Immutable & verifiable**|**No data can be changed or deleted**, and it’s **cryptographically secure**.|
|⚙️ **Fully managed & serverless**|AWS takes care of everything — no servers or maintenance needed.|
|🌐 **High availability**|Data is **automatically replicated across 3 Availability Zones**.|
|🧠 **SQL interface**|You can use **SQL** to query and interact with data.|
|🚀 **High performance**|**2–3x faster** than traditional blockchain ledger systems.|

---

### 🔸 Real-Life Example:

A bank wants to **track every update to customer balances** — including when, why, and by whom —  
but **no one should be able to change or delete** past records.

**Amazon QLDB**:

- Records all events **forever**
    
- Prevents tampering
    
- Ensures data can be **audited anytime**
  
  ## Amazon Managed Blockchain

**Amazon Managed Blockchain** is a **fully managed service** that helps you create and manage **blockchain networks** where **multiple parties can share data and run transactions** **without needing a central authority**.

---

### 🔑 Key Features (Simplified):

|Feature|Explanation|
|---|---|
|🧱 **Blockchain-based apps**|Lets you build apps where **multiple organizations can work together** securely.|
|⚙️ **Fully managed**|AWS handles setup, scaling, and maintenance of blockchain networks.|
|🔄 **Join or create networks**|You can either **join public blockchains** (like Ethereum) or **create private ones** (like Hyperledger Fabric).|
|🛠️ **Supports major frameworks**|Works with **Hyperledger Fabric** (private, permissioned) and **Ethereum** (public, decentralized).|

---

### 🔸 Real-Life Example:

A supply chain with **multiple companies** wants to:

- Track a product from manufacturer → distributor → retailer
    
- Make sure **each step is verified** and **no one can cheat**
    

With **Amazon Managed Blockchain**:

- Each company joins the **same blockchain network**
    
- All transactions are recorded in a **tamper-proof, shared ledger**
    
- There’s **no need for a central admin**
    

---

### 🔁 Difference from Amazon QLDB:

|Feature|**Amazon Managed Blockchain**|**Amazon QLDB**|
|---|---|---|
|🏛️ **Trust model**|**Decentralized**, no central owner|**Centralized**, owned by one authority (you)|
|🧠 **Use case**|Multi-party collaboration (e.g., supply chain)|Internal systems (e.g., audit logs, finance)|
|🧱 **Blockchain framework**|Uses **Ethereum** or **Hyperledger Fabric**|Not a blockchain — uses ledger structure|
|🔐 **Immutability**|Yes (via blockchain)|Yes (via cryptographic hash chain)|
## **AWS Glue**

**AWS Glue** is a **fully managed, serverless ETL** (Extract, Transform, Load) service.  
It helps you **prepare and move data** from one place to another — ready for **analytics**.

---

### 🧪 What does ETL mean?

|Step|What It Does|
|---|---|
|🧲 **Extract**|Pull data from sources like **S3 or Amazon RDS**|
|🔧 **Transform**|Clean, filter, or reformat the data|
|📤 **Load**|Send it to a destination like **Amazon Redshift** for analysis|

---

### 🔍 Diagram Explained:

1. **Data Sources**:
    
    - From **Amazon S3** or **Amazon RDS**
        
    - AWS Glue **extracts** raw data from these
        
2. **Glue ETL Job**:
    
    - Data is **transformed** (cleaned, enriched, or reformatted)
        
3. **Destination**:
    
    - Transformed data is **loaded** into **Amazon Redshift**, where it can be queried efficiently
        

---

### 📚 Glue Data Catalog

- It's a **metadata store** (like an index of all your data)
    
- Can be used by **Athena, Redshift, EMR** to understand and query your data
    

---

### ✅ Use Cases:

- Preprocessing logs for analysis
    
- Cleaning data before loading into a data warehouse
    
- Automating daily ETL pipelines
  
  ## AWS DMS

**AWS Database Migration Service (DMS)** helps you **move databases to AWS** quickly, securely, and **with minimal downtime**.  
The **source database stays active** during the migration, so your applications keep running.

---

### 🔑 Key Features (Simplified):

|Feature|Explanation|
|---|---|
|🚀 **Fast & secure migration**|Move data to AWS **without stopping** your apps|
|🔄 **Live replication**|Keeps syncing data **even while users are using it**|
|💪 **Resilient & self-healing**|Automatically restarts on failure and keeps migrating|
|🔁 **Supports both types**|✅ **Homogeneous**: Same DB engine (e.g., Oracle → Oracle)|

pgsql

Copy code

                              `✅ **Heterogeneous**: Different engines (e.g., SQL Server → Aurora) |`

---

### 🔸 Real-Life Example:

You want to **move your on-premise Microsoft SQL Server** database to **Amazon Aurora**:

- Without DMS: You’d have to stop your app and copy everything manually (risky + downtime).
    
- With **DMS**:
    
    - Your **existing database keeps working**.
        
    - DMS **copies everything in the background**.
        
    - After final sync, you switch over — **no disruption** to users.

## Docker

**Docker** is a tool that lets you **package your app and everything it needs** (code, libraries, dependencies) into a single unit called a **container**.

This container can run **anywhere** — on **any machine**, **any operating system**, **cloud** or **local laptop** — and the app will always behave **exactly the same**.

---

### 🔑 Key Benefits (Simplified):

|Feature|Why It Matters|
|---|---|
|📦 **Containers**|Like lightweight, portable mini-servers for your app|
|🔄 **Works anywhere**|Same app behavior on Windows, Mac, Linux, AWS, etc.|
|💻 **No compatibility issues**|No "works on my machine" problems|
|⚙️ **Faster deployment**|Start/stop apps in **seconds**, not minutes or hours|
|🧹 **Easier to maintain**|All dependencies are included — fewer bugs from version mismatches|
|🌐 **Any language/tech**|Works with Python, Node.js, Java, .NET, etc.|
|🚀 **Scalable**|Run 1 or 1000 containers depending on need — scales easily|

---

### 🔸 Real-Life Example:

You build a **Python web app** that needs a specific version of Python, Flask, and a database driver.

- Instead of configuring every machine manually...
    
- You create a **Docker container** with everything pre-installed.
    
- Now anyone (or any server) can run your app with **one command** — and it’ll always work.

## Docker Images?

A **Docker Image** is like a **blueprint or snapshot** of an app.  
It includes:

- The **app’s code**
    
- All its **dependencies** (like libraries, runtimes)
    
- **Instructions** on how to run it
    

👉 Think of it like a **recipe** for making a container.

When you run a Docker image, it becomes a **container** — a running version of your app.

---

### 🔸 Example:

If you're building a Node.js app:

- Your Docker image might include: **Node.js**, your **JavaScript files**, and **npm packages**
    
- Once built, you can run it **anywhere**, and it behaves the same
    

---

## 📦 Where Are Docker Images Stored?

|Type|Description|
|---|---|
|🌍 **Public Repositories**|Like a global app store for Docker images|
|👉 **Docker Hub**|[https://hub.docker.com](https://hub.docker.com) — official & community images|
|🔐 **Private Repositories**|For secure, private image storage|
|🛡️ **Amazon ECR**|**Elastic Container Registry** — AWS's managed Docker image storage|

---

### 🔁 In Summary:

|Term|Meaning|
|---|---|
|**Docker Image**|A package with your app and everything it needs|
|**Container**|A running instance of a Docker image|
|**Repository**|A storage location for Docker images|
|**Docker Hub**|Public image storage for everyone|
|**Amazon ECR**|Private image storage on AWS|
## Docker vs Virtual Machines – Explained:

### 🚀 Virtual Machines (Left Side of Diagram)

- Each **App** runs inside its **own Guest OS**.
    
- Guest OSes run on top of a **Hypervisor**, which runs over the **Host OS** and **Infrastructure**.
    
- Each VM has:
    
    - Its own **operating system**
        
    - More **overhead** and **slower startup**
        
    - Uses **more memory/CPU**
        

🧠 Think of it like:

> **Each app has its own full computer (including its own OS)** — more isolated but heavier.

---

### 🐳 Docker Containers (Right Side of Diagram)

- All containers share the **same Host OS** and use the **Docker Daemon**(The **Docker Daemon** (`dockerd`) is the **core background service** in Docker that **runs on your system**.)to run.
    
- No need for separate Guest OS for each app.
    
- **Lightweight**, **faster**, and **uses fewer resources**.
    

🧠 Think of it like:

> **Multiple apps running in lightweight containers** on one shared OS — less isolated but faster and more efficient.

---

## 🔁 Summary Table:

|Feature|**Virtual Machines**|**Docker Containers**|
|---|---|---|
|OS per app|Yes (Guest OS for each VM)|No (shared Host OS)|
|Startup time|Minutes|Seconds|
|Resource usage|High (more RAM, CPU)|Low (lightweight)|
|Isolation level|High (full OS isolation)|Medium (process-level isolation)|
|Use case|Different OSes, strong security, legacy apps|Fast app deployment, microservices, CI/CD|

---

✅ **Conclusion** from the slide:  
Docker is _"sort of"_ a virtualization tech, but **lighter and more efficient** than VMs.  
It lets you run **many containers on one server** because it **shares resources with the host OS**.

## ECS

**ECS (Elastic Container Service)** is an AWS service that helps you **run Docker containers** in the cloud.

---

### 🔧 Key Points (Simplified):

|Feature|What It Means|
|---|---|
|🚀 **Runs Docker containers**|Easily launch and manage your **containerized apps** on AWS|
|🖥️ **You manage EC2 instances**|You need to **set up and maintain the servers** (EC2 instances)|
|🤖 **AWS manages containers**|ECS handles **starting, stopping, and restarting** containers|
|🔗 **Load Balancer support**|Works with **Application Load Balancer** to distribute traffic|

---

### 🔸 Real-Life Example:

You have a **web app in a Docker container** and want to deploy it to AWS.  
With **ECS**, you:

- Launch a few EC2 instances
    
- Let ECS place your containers on them
    
- Use a Load Balancer to route traffic
    

✅ Result: Your app is running and auto-scaled, but **you manage the EC2 infrastructure**.


## Fargate

**AWS Fargate** is a **serverless way to run Docker containers** —  
**no EC2 instances to manage**, and AWS takes care of everything behind the scenes.

---

### 🔧 Key Points (Simplified):

|Feature|What It Means|
|---|---|
|🚫 **No EC2 setup needed**|You **don’t manage servers** — just tell AWS what CPU/RAM you need|
|🧠 **Serverless containers**|AWS runs your containers **automatically**|
|⚡ **Simpler than ECS on EC2**|Focus only on your app, not infrastructure|
|📦 **Use cases**|Great for microservices, jobs, APIs, and quick-deploy apps|

---

### 🔸 Real-Life Example:

You want to deploy a Python app in a Docker container:

- With **ECS on EC2** → You must launch & maintain EC2 servers
    
- With **Fargate** → You just define how much CPU/RAM your container needs  
    ✅ AWS runs it **automatically** — no server headaches
## (ECR ) Elastic Container Registry 

• Private Docker Registry(a **registry** is a **secure storage location** where your **Docker images** are stored.) on
AWS

• This is where you store your
Docker images so they can
be run by ECS or Fargate

## What is Kubernetes?

**Kubernetes** (often called **K8s**) is an **open-source platform** for:

- **Managing**, **deploying**, and **scaling** containerized applications (like Docker apps)
    
- Automating **container orchestration** (which container runs where, when, and how many)
    

---

### 🔧 Think of Kubernetes like:

> 🧠 **A smart manager** for running 100s or 1000s of containers across machines (servers)  
> It decides **what runs where**, **monitors health**, **restarts crashed containers**, and **scales automatically**

---

## 🐳 Example:

You have a **Docker container** that runs a web app.

Using Kubernetes, you can:

- Define how many copies (replicas) should run
    
- Tell it what to do if one crashes
    
- Auto-scale if traffic increases
    
- Load balance across containers

## Amazon EKS

**Amazon EKS** = **Managed Kubernetes Service** on AWS.

|Without EKS|You install & manage Kubernetes manually on EC2 machines|
|---|---|
|With **Amazon EKS**|AWS sets up and manages Kubernetes control plane for you — less work|

---

### 🔑 Key Features of Amazon EKS:

|Feature|Explanation|
|---|---|
|🔧 **Managed Kubernetes**|AWS runs the Kubernetes core (control plane) for you|
|📦 **Runs Docker containers**|Works with containerized apps like microservices|
|☁️ **Cloud-agnostic**|Kubernetes can work on AWS, GCP, Azure, or on-prem too|
|🖥️ **Flexible hosting options**|Run containers on EC2 (your infra) or Fargate (serverless)|

---

### 🔁 EKS vs ECS vs Fargate:

|Feature|ECS|EKS (Kubernetes)|Fargate|
|---|---|---|---|
|Control|AWS-specific|Open-source, cloud-agnostic|Serverless backend|
|Flexibility|Simpler, opinionated|More flexible, complex|Used with ECS/EKS|
|Use case|AWS-only container use|Cross-platform, complex apps|No infra to manage|
## Serverless

**Serverless** is a way of building and running applications **without managing servers**.

➡️ **You focus on your code**,  
➡️ **AWS handles the servers** (provisioning, scaling, patching, etc.)

---

### 🧠 Important Clarification:

> 🔸 **“Serverless” doesn’t mean there are no servers**  
> It means **you don’t manage** or even see them — AWS does that for you.

---

## 🔧 How It Works (Simplified):

Instead of launching servers, you:

- Just write a **function** or small piece of code
    
- Deploy it to a **serverless platform** like **AWS Lambda**
    
- AWS runs it **only when needed**
    
- You pay **only for the time it runs**
    

---

## 🔥 Real-Life Example:

You want a function that runs when a user uploads a file to S3:

- You write a short code (function)
    
- Deploy it using **AWS Lambda**
    
- When a file is uploaded → Lambda **automatically runs your function**
    
- No servers to set up or manage
  
| Feature              | **Amazon EC2**                                   | **AWS Lambda**                                         |
| -------------------- | ------------------------------------------------ | ------------------------------------------------------ |
| **Type**             | Virtual **Server**                               | Virtual **Function** (Serverless)                      |
| **Management**       | You manage servers, OS, updates, etc.            | AWS manages everything – no servers to manage          |
| **Runtime Duration** | **Runs continuously**                            | **Runs on demand**, up to **15 minutes per execution** |
| **Scaling**          | Manual or Auto Scaling (needs setup)             | **Automatic** scaling built-in                         |
| **Billing**          | Pay per hour/second, **even if idle**            | Pay **only when code runs**                            |
| **Startup Time**     | Slower (boot OS, services)                       | Fast (milliseconds)                                    |
| **Use Cases**        | Websites, long-running apps, custom environments | Event-driven tasks, APIs, automation, short jobs       |
| **Resource Limits**  | Based on chosen instance type (RAM/CPU)          | Limited (e.g. 10GB RAM, 15 mins max runtime)           |
| **OS Control**       | Full control (you choose OS, software)           | No OS-level control (just run your function code)      |


### **API**

**API** stands for **Application Programming Interface**.

It is a **set of rules** that allows **two software systems to talk to each other**.

---

### 🔧 Simple Definition:

> An **API is like a waiter in a restaurant** 🍽️  
> You (the user) tell the waiter what you want (e.g., “1 burger”),  
> The waiter goes to the kitchen (backend), gets it, and brings it back.

---

### 🧠 In Tech Terms:

- You write an app (like a weather app)
    
- It uses an **API** to request weather data from a weather service
    
- The weather service sends back data (like temperature) through the API

## Amazon API Gateway?

**Amazon API Gateway** is a **fully managed service** that helps you:

- Create and expose **APIs** (Application Programming Interfaces)
    
- Connect those APIs to **backend services**, like **AWS Lambda**, **EC2**, or **databases**
    
- Handle **security**, **monitoring**, and **traffic control**
    

---

### 🔧 Key Features (Simplified):

|Feature|Explanation|
|---|---|
|🔗 **Connects frontend to backend**|Acts as the “gate” between clients (web/mobile apps) and your services|
|⚡ **Serverless & scalable**|No infrastructure to manage; it auto-scales|
|🔁 **Supports REST & WebSocket**|Build normal HTTP APIs or real-time chat-style APIs|
|🔐 **Security & Auth**|Supports IAM, Cognito, API keys, throttling, usage plans, etc.|
|📊 **Monitoring**|Integrated with CloudWatch for performance and usage tracking|

---

### 🔸 Real-Life Example:

You build a **todo app** where the frontend calls an API like:
This API:

- Is created using **API Gateway**
    
- Triggers an **AWS Lambda function** to store the task in **DynamoDB**
    
- Handles **authentication**, **rate-limiting**, and **monitoring** for you
    

✅ Result: **Secure, scalable, serverless API** with **zero server management**

## AWS Batch

**AWS Batch** is a **fully managed service** that lets you run **batch processing jobs** (jobs that have a start and end) **at any scale** — without managing infrastructure.

---

### 🔧 Key Features (Simplified):

|Feature|Meaning|
|---|---|
|✅ **Managed by AWS**|No need to manually launch servers – AWS does it for you|
|🚀 **Runs batch jobs**|Ideal for jobs that **start, run, finish** (not continuous services)|
|🖥️ **Auto-provisions EC2/Spot**|Automatically chooses the right compute (EC2 or cheaper Spot instances)|
|🐳 **Docker support**|Each job is a **Docker image**, run on **ECS** under the hood|
|⏱️ **Scheduled or manual**|You can **submit jobs or schedule them** to run at certain times|
|💰 **Cost-effective**|Uses Spot Instances when possible to save money|

---

### 🔸 Real-Life Example:

You have a task to **convert 10,000 images to grayscale**:

- You write a Python script that does the job
    
- Package it in a **Docker container**
    
- Submit it as a **batch job** to AWS Batch

### AWS Lambda vs AWS Batch

|Feature|**AWS Lambda**|**AWS Batch**|
|---|---|---|
|🕒 **Time Limit**|Yes – Max **15 minutes per execution**|❌ **No time limit**|
|🔧 **Runtime Support**|Limited to supported runtimes (e.g., Python, Node.js)|**Any language/runtime** via Docker container|
|💾 **Disk Space**|Limited (~512MB of /tmp space)|Uses **EBS or instance storage** (can be large)|
|⚙️ **Infrastructure**|Fully **serverless**, no servers to manage|Runs on **EC2 instances** (managed by AWS)|
|🐳 **Docker Support**|Optional (via Lambda container images)|Required – **jobs must be Dockerized**|
|🔁 **Ideal For**|Short, event-driven tasks|Long-running, compute-heavy, or scheduled batch jobs|
|💰 **Pricing**|Pay per request and runtime duration|Pay for EC2/Spot compute resources used|
|⏱️ **Use Case Examples**|Image resizing, API calls, lightweight automation|Data processing, video rendering, simulations, large loops|
## Amazon Lightsail?

**Amazon Lightsail** is an **easy-to-use cloud platform** that provides everything you need to launch and manage simple apps and websites — **with low cost and minimal setup**.

---

### 🔧 Key Features (Simplified):

|Feature|What It Means|
|---|---|
|🖥️ **Virtual servers**|Launch your own servers (like EC2) easily|
|💾 **Includes storage & databases**|Comes with storage (like EBS) and managed DBs (like RDS)|
|🌐 **Simplified networking**|Built-in networking features (static IPs, DNS, firewalls)|
|💸 **Fixed pricing**|Cheaper and **predictable monthly billing**|
|🧰 **Templates available**|One-click install: WordPress, Node.js, LAMP stack, Joomla, etc.|
|🛠️ **Best for beginners**|Easy UI, no deep AWS knowledge required|

---

### ✅ Use Cases:

- Simple websites & blogs (e.g., WordPress)
    
- Web apps using Node.js, LAMP, MEAN, etc.
    
- Dev/Test environments for small teams
    
- Small business websites or e-commerce (Magento)
    

---

### 🔁 Lightsail vs EC2 (Quick View):

| Feature      | **Lightsail**                  | **EC2**                              |
| ------------ | ------------------------------ | ------------------------------------ |
| Ease of use  | Simple UI, good for beginners  | More control, steeper learning curve |
| Pricing      | Fixed monthly                  | Pay-per-use (variable)               |
| Auto-scaling | ❌ No auto-scaling              | ✅ Supports auto-scaling              |
| Integrations | Limited AWS integrations       | Full AWS service integration         |
| Use case     | Simple apps/websites, dev/test | Production-level, complex systems    |
### AWS CloudFormation

**AWS CloudFormation** is a tool that lets you **define your entire AWS infrastructure using code** — instead of clicking around manually in the AWS console.

---

### 🧠 Simple Explanation:

> Think of CloudFormation as a **"blueprint"** or **"recipe"** for your cloud setup.

You write a **template file** (in YAML or JSON) saying:

- I want **1 S3 bucket**
    
- I want **2 EC2 instances**
    
- I want a **load balancer**
    
- I want a **security group**
    

CloudFormation **reads your template** and **creates everything automatically** — in the correct order.

---

### 📦 Key Features:

|Feature|Explanation|
|---|---|
|📝 **Declarative**|You declare **what** you want, not **how** to build it|
|🔁 **Reusable templates**|Reuse templates to spin up environments consistently|
|🧱 **Any AWS resource**|Supports most AWS services (EC2, S3, RDS, Lambda, etc.)|
|🔄 **Automated dependency mgmt**|Knows the **order to create resources** (e.g., security group before EC2)|
|♻️ **Easily update or delete**|Update or delete entire infrastructure from the template|

---

### 🔸 Real-Life Use Case:

You need a test environment with:

- 1 VPC
    
- 2 EC2 instances
    
- 1 Load Balancer
    

Instead of manually setting it up:  
✅ Write a CloudFormation template  
✅ Deploy it → AWS builds it all for you  
✅ Reuse the same template anytime

## **Benefits of AWS CloudFormation (1/2)**

### 1️⃣ **Infrastructure as Code (IaC)**

- You write your AWS infrastructure in **code** (YAML or JSON)
    
- No manual creation → **better consistency and repeatability**
    
- All changes can be **reviewed, tracked, and version-controlled**
    

---

### 2️⃣ **Cost Visibility & Control**

- Every resource is **tagged** → easy to **track costs per stack**
    
- You can **estimate total cost** before deploying (via template)
    
- Helps identify and avoid **unexpected expenses**
    

---

### 3️⃣ **Automation for Savings**

- In **dev environments**, you can:
    
    - Automatically **delete stacks at 5 PM**
        
    - Recreate them at **8 AM next day**
        
- Saves money by **not keeping unused resources running overnight**

### 4️⃣ **Productivity Boost**

- You can **quickly destroy and re-create** entire infrastructure setups
    
- Great for **testing, development, or resetting environments**
    

---

### 5️⃣ **Automatic Diagrams**

- CloudFormation can **auto-generate diagrams** of your architecture from the template  
    🔍 Helpful for **visualizing** what will be created
    

---

### 6️⃣ **Declarative Programming**

- You only specify **what you want** (e.g., “I want an EC2 instance”)
    
- CloudFormation figures out **how and in what order** to build it  
    ✅ No need to worry about orchestration logic
    

---

### 7️⃣ **Don’t Re-invent the Wheel**

- Many **ready-made templates** are available online for common setups
    
- You can copy, modify, or combine them — **saves tons of time**
    

---

### 8️⃣ **Great Documentation**

- AWS provides **clear docs and examples** to guide you  
    📘 Easy to learn, use, and troubleshoot

## AWS CDK

**AWS CDK (Cloud Development Kit)** lets you **write AWS infrastructure using real programming languages** like:

- **Python**
    
- **JavaScript/TypeScript**
    
- **Java**
    
- **.NET (C#)**
    

Instead of writing long YAML or JSON CloudFormation templates, you write **normal code** — which the CDK then **converts into a CloudFormation template** and deploys.


### Key Features:

|Feature|Explanation|
|---|---|
|🧑‍💻 **Familiar Languages**|Write infra in Python, JS, Java, etc.|
|📦 **Built-in Constructs**|Reusable components (like L3 constructs for S3, Lambda)|
|🔄 **Infra + App together**|Deploy **Lambda functions**, **Docker containers**, etc., along with infra|
|🚀 **Simplifies CloudFormation**|You avoid writing raw YAML/JSON|
|📁 **Reusable & testable**|Use functions, loops, and logic to make your infra smarter|

---

### ✅ Best Use Cases:

| Use Case                 | Why CDK Works Well                                    |
| ------------------------ | ----------------------------------------------------- |
| 🧠 Lambda Functions      | Define the function + trigger + IAM in one code block |
| 🐳 ECS / EKS with Docker | Easily configure Docker images and services           |
| 📦 CI/CD Pipelines       | Integrate infra as code with dev pipelines            |
## Developer problems on AWS

• Managing infrastructure
• Deploying Code
• Configuring all the databases, load balancers, etc
• Scaling concerns
• Most web apps have the same architecture (ALB + ASG)
• All the developers want is for their code to run!
• Possibly, consistently across different applications and environments

## What is **Elastic Beanstalk**?

**AWS Elastic Beanstalk** is a **fully managed service** that helps you **deploy and run web applications** easily — without worrying about the infrastructure.

> 💡 You just upload your **code** → Beanstalk handles the rest (servers, scaling, monitoring, etc.)

---

### 🔧 Key Features:

|Feature|What It Means|
|---|---|
|⚙️ **Managed infrastructure**|You don’t need to set up EC2, load balancer, auto-scaling, etc. — Beanstalk does|
|🛠️ **Supports multiple languages**|Java, Python, Node.js, PHP, .NET, Ruby, Go, Docker|
|🔁 **Auto-scaling & Load balancing**|Automatically adjusts based on traffic|
|📊 **Monitoring built-in**|Tracks app health & responsiveness|
|🗂️ **Configurable deployments**|Choose how updates are rolled out (all-at-once, rolling, etc.)|

---

### 📦 What **you manage** vs **Beanstalk manages**:

|Task|Who Handles It|
|---|---|
|Writing app code|✅ **You**|
|Creating servers (EC2)|✅ **Beanstalk**|
|Load balancer setup|✅ **Beanstalk**|
|Auto-scaling|✅ **Beanstalk**|
|Monitoring app health|✅ **Beanstalk**|
|Updating OS / patches|✅ **Beanstalk**|

---

### 🧱 Architecture Models:

|Model|Use Case|
|---|---|
|🧪 **Single Instance**|Good for **development/testing**|
|🌐 **Load Balancer + ASG**|Best for **production web apps**|
|⚙️ **ASG only (no LB)**|Great for **non-web apps** (like workers)|

---

### ✅ Summary:

> **Elastic Beanstalk** = Simplified app deployment on AWS  
> Just write your **app code**, and AWS handles the **infra, scaling, and monitoring**

## AWS CodeDeploy?

**AWS CodeDeploy** helps you **automatically update your app** on:

- 🖥️ **EC2 instances**
    
- 🏠 **On-premise servers** (your own computers/servers)
    

---

## ✅ Why use it?

- You **don’t need to update apps manually**
    
- It pushes **new code** to servers **automatically**
    
- Works in both **cloud and local environments**
    

---

## 🔧 What you need:

- Your servers must have a **CodeDeploy agent** (a small setup file)
    
- Your app is stored in **S3 or GitHub**
    
- You define **how the update should happen**
### Key Features:

|Feature|Meaning|
|---|---|
|✅ **Automatic deployments**|No manual file copying or restarts — CodeDeploy handles it|
|💻 **Works with EC2**|Can update apps on virtual servers in AWS|
|🏠 **Works on-premises too**|Can push updates to your physical or local servers|
|🔁 **Hybrid**|Mix of cloud + on-premise deployments supported|
|🧑‍💻 **Agent-based**|You install **CodeDeploy Agent** on target servers to enable deployment|
|🔄 **Update strategies**|Supports **in-place** (update live server) and **blue/green** (new server) deployments|
## AWS CodeBuild?

**AWS CodeBuild** is a service that:

> ✅ **Builds your code in the cloud**  
> ✅ **Runs tests**  
> ✅ **Creates deployable files (like `.zip`, `.jar`, etc.)**

---

## 🔧 Why use CodeBuild?

- You don’t need your own **build server**
    
- AWS handles everything: **build tools, scaling, and security**
    
- Works well with **CodeDeploy**, **CodePipeline**, etc.
    

---

### 💡 Real-Life Example:

Let’s say you push new code to GitHub:

1. **CodeBuild** compiles the code
    
2. **Runs unit tests**
    
3. Creates a final package
    
4. Sends it to **CodeDeploy** to update the app
    

✅ All this happens automatically!

---

## 🌟 Key Benefits:

|Feature|What it means|
|---|---|
|🧰 **Fully managed**|You don’t worry about servers|
|⚙️ **Serverless**|No setup, just connect your code repo|
|📈 **Auto-scalable**|Can handle one or thousands of builds|
|🔒 **Secure**|Integrated with AWS IAM and encryption|
|💰 **Pay-as-you-go**|Only pay for the time your build runs|
### Summary:

> **CodeBuild = Online code builder**  
> It compiles, tests, and prepares your app — ready for deployment.

## AWS CodePipeline?

**AWS CodePipeline** is a **fully managed CI/CD ( Continuous Integration / Continuous Delivery)

- **CI (Continuous Integration):**  
    Every time you push code, it’s **automatically tested and built**.
    
- **CD (Continuous Delivery):**  
    The tested code is then **automatically deployed** to your app or server.)
    
service** that **automates the steps to deliver your code** from development to production.

> 💡 Think of it as an **assembly line** for your code:  
> **Write Code → Build → Test → Deploy → Done!**

---

## ⚙️ What Does It Do?

|Stage|What Happens|
|---|---|
|🧑‍💻 **Code**|You push code to GitHub or CodeCommit|
|🏗️ **Build**|CodeBuild compiles & runs tests|
|📦 **Test**|Optional tests like unit or integration tests|
|🚀 **Deploy**|CodeDeploy or Beanstalk deploys it live|

---

## ✅ Benefits:

|Feature|Meaning|
|---|---|
|🔁 **Fully automated**|No manual steps needed|
|⚙️ **Fully managed**|AWS handles the infrastructure|
|🔌 **Integrates easily**|Works with GitHub, CodeCommit, CodeBuild, CodeDeploy, etc.|
|⚡ **Fast delivery**|Quickly release new features or bug fixes|
|🧩 **Supports custom plugins**|Add extra actions like security scans or notifications|

---

## 🧠 Real-Life Example:

You update code in GitHub →  
✅ CodePipeline automatically triggers:

1. **Build** → via CodeBuild
    
2. **Test** → your unit tests
    
3. **Deploy** → via CodeDeploy or Beanstalk

## AWS CodeArtifact

**AWS CodeArtifact** is a **cloud service that stores code packages and libraries** — so developers can **reuse code** and manage **dependencies** easily.

---

### 🧠 Why it’s useful:

When you build software, your app needs other code (called **dependencies**), like:

- `express` in Node.js
    
- `numpy` in Python
    
- `spring-core` in Java
    

Instead of downloading these from the internet every time…

> ✅ **CodeArtifact stores them in one safe place**, so your team or AWS CodeBuild can use them quickly and securely.

---

### 🛠️ Key Features:

|Feature|Meaning|
|---|---|
|📦 **Artifact management**|Stores reusable code packages and libraries|
|🛡️ **Secure & scalable**|AWS handles storage, access, and scaling|
|🔄 **Integrates with tools**|Works with npm, pip, Maven, Gradle, NuGet, etc.|
|🧪 **Used by CodeBuild**|CodeBuild can download dependencies from it|

---

### 💡 Real-Life Example:

You're building a Node.js app with `npm`.  
Instead of pulling packages from public `npmjs.com`, you can:

1. Upload your **own custom packages** to **CodeArtifact**
    
2. Let **developers or CodeBuild** pull packages from **CodeArtifact** directly
    

---

### ✅ Summary:

> **AWS CodeArtifact = Private app store for your code packages**  
> Helps teams **store, share, and use code dependencies** in a safe and fast way.

## AWS Systems Manager (SSM)?

**AWS Systems Manager (SSM)** is a **tool to help you manage lots of servers easily**, whether they’re:

- On **AWS (like EC2)**
    
- Or **your own physical servers** (on-premises)
    

✅ It works for **Linux, Windows, macOS, and even Raspberry Pi**.

---

### 🔧 Why use it?

> Imagine managing 100+ servers. Instead of logging into each one manually…

✅ **SSM lets you control, monitor, and automate tasks** across **all of them at once**.

---

### 🌟 Key Features (in simple terms):

|Feature|What it does|
|---|---|
|🔄 **Patching Automation**|Keeps all your servers up-to-date with latest security patches|
|🖥️ **Run Commands Remotely**|Run scripts/commands on multiple servers at once|
|🔐 **Parameter Store**|Safely store config data like passwords, DB URLs, etc.|
|📊 **Operational Insights**|See the health/status of your infrastructure|

---

### 🧠 Real-Life Example:

You have 50 EC2 servers and need to:

- Install updates
    
- Run a health check script
    
- Update an environment variable
    

✅ SSM can do all of that from **one dashboard**, with **one click or command**.

---

### ✅ Summary:

> **AWS SSM = Remote control + automation for all your servers**  
> Helps you **patch, monitor, and manage** EC2 and on-prem systems — **easily and securely**.

# Global Infrastructure Section 
## Why Build a Global Application?

A **global application** means your app is **deployed in multiple parts of the world** — like **Asia, Europe, US, etc.**

---

### ✅ Benefits:

|Reason|Why It Matters|
|---|---|
|⚡ **Faster for users**|Hosting near users = **less delay (latency)** = quicker load time|
|🌐 **Better user experience**|People in different countries get fast and smooth performance|
|🛡️ **Disaster Recovery (DR)**|If one region fails (earthquake, power cut), another region keeps app running|
|🔒 **Harder to attack**|A global setup is more secure than one single server or region|

---

### 💡 Real-Life Example:

If your users are in India and your server is only in the US:

- Requests will be **slow** (long distance = high latency)
    

If you deploy the app in **Mumbai and Frankfurt**:

- Indian users hit Mumbai server (faster)
    
- European users hit Frankfurt server (faster)
    
- ✅ Everyone gets better performance
    

---

### ✅ Summary:

> A **global application** gives your users **fast access**, **high availability**, and **better protection** — no matter where they are in the world.


## AWS Global Infrastructure = 3 Key Layers

|Part|What It Is|Purpose|
|---|---|---|
|🌍 **Regions**|Big geographic areas (like Mumbai, Tokyo, Ohio)|Where you deploy your applications|
|🏢 **Availability Zones (AZs)**|Multiple **data centers** inside a region|Help with **high availability** and failover|
|📍 **Edge Locations**|Small data centers close to users|Used for **faster content delivery** (via CloudFront/CDN)|

---

### 🧠 Real Example:

Say you host your website in **Mumbai (Region)**:

- It uses **3 Availability Zones** for reliability
    
- Your videos/images are cached in **Edge Locations** near users in **Delhi or Chennai** for speed

## Amazon Route 53 Overview

• Route53 is a Managed DNS (Domain Name System)

• DNS is a collection of rules and records which helps clients understand
how to reach a server through URLs.

## Amazon CloudFront?

**Amazon CloudFront** is a **Content Delivery Network (CDN)** —  
It **delivers your website content faster** by storing (caching) it in **multiple locations around the world** 🌍.

---

### ⚡ Why Use CloudFront?

|Benefit|What It Means|
|---|---|
|🚀 **Faster Performance**|Content is cached closer to users at **edge locations**|
|😊 **Better User Experience**|Websites, videos, and apps load faster|
|🛡️ **Security**|Protects from DDoS attacks with **AWS Shield + WAF integration**|
|🌐 **Global Reach**|216+ **edge locations** around the world|

---

### 💡 Example:

Your main website is hosted in **Mumbai**, but someone in **London** visits it.

> Instead of reaching Mumbai, CloudFront shows a **cached version** from an edge location near London.  
> ✅ **Faster loading + less server load**

---

### ✅ Summary:

> **Amazon CloudFront = Global Speed Booster + Protector for your content**  
> It caches content at edge locations, improves performance, and adds DDoS security

## CloudFront – Origins
### • S3 bucket
• For distributing files and caching them at the edge
• Enhanced security with CloudFront Origin Access Control (OAC)
• OAC is replacing Origin Access Identity (OAI)
• CloudFront can be used as an ingress (to upload files to S3)
### • Custom Origin (HTTP)
• Application Load Balancer
• EC2 instance
• S3 website (must first enable the bucket as a static S3 website)
• Any HTTP backend you want

## CloudFront vs S3 Cross Region Replication
### • CloudFront:
• Global Edge network
• Files are cached for a TTL (maybe a day)
• Great for static content that must be available everywhere

### • S3 Cross Region Replication:
• Must be setup for each region you want replication to happen
• Files are updated in near real-time
• Read only
• Great for dynamic content that needs to be available at low-latency in few regions


S3 Transfer Acceleration
• Increase transfer speed by transferring file to an AWS edge location which will forward the data to the S3 bucket in the target region

## What is AWS Global Accelerator?

**AWS Global Accelerator** makes your global application:

- **Faster** 🌍
    
- **More reliable** 🔒
    
- **Easier to access from anywhere** 📶
    

It does this by using **AWS's private global network** instead of the slower public internet.

---

### ⚙️ How it Works:

1. You get **2 static IPs** (called **Anycast IPs**).  
    These never change and are used by all users, no matter where they are.
    
2. When someone uses your app:
    
    - The request first goes to the **nearest AWS Edge Location**.
        
    - Then it travels through **AWS's fast internal network** to reach your application (on EC2, ALB, etc).
        

✅ This is much faster than normal internet routing.

---

### 📈 Benefits:

|Feature|What It Means|
|---|---|
|⚡ **Faster performance**|Up to **60% faster** routing than public internet|
|🔒 **More reliable**|If one region/server fails, traffic is rerouted fast|
|🌍 **Global access**|Same IPs for all users around the world|
|🛣️ **Optimized routing**|Picks the **best AWS route** automatically|
## AWS Global Accelerator vs CloudFront
• They both use the AWS global network and its edge locations around the world
• Both services integrate with AWS Shield for DDoS protection.
##### • CloudFront – Content Delivery Network
• Improves performance for your cacheable content (such as images and videos)
• Content is served at the edge
###### • Global Accelerator
- ❌ **No caching**: It doesn’t store content like CloudFront
    
- ✅ It works like a **smart traffic controller**, **forwarding packets (data)** directly to your app in AWS Regions

### ⚡ **Performance Improvements**

- Speeds up all kinds of apps using **TCP or UDP** (common internet communication types)

- Especially useful for apps that **must respond quickly** and **reliably** — like real-time apps, games, or APIs
- • Good for HTTP use cases that require static IP addresses • Good for HTTP use cases that required deterministic, fast regional failover
  
  ### ✅ **AWS Outposts – Explained Simply**

- **Hybrid cloud** = Use both **AWS Cloud** and your **own local servers**
    
- Normally, you manage cloud and on-prem systems **separately**
    

---

### 🖥️ **What is AWS Outposts?**

- AWS installs **server racks** (called Outposts) in your **data center**
    
- Lets you run **AWS services locally** with the **same tools/APIs** as the cloud
    
- AWS manages the hardware, **you handle physical security**
    

---

### 📌 Use Case:

> When you want **cloud power** but need data/apps to run **on-site**


AWS Outposts
• Benefits:
• Low-latency access to on-premises systems
• Local data processing
• Data residency
• Easier migration from on-premises to the cloud
• Fully managed service
• Some services that work on Outposts:
Amazon EC2 Amazon EBS Amazon S3 Amazon EKS Amazon ECS Amazon RDS Amazon EMR

### **AWS Wavelength – Simple Explanation**

- **Wavelength Zones** are AWS infrastructure inside **telecom (5G) data centers**
    
- Brings **AWS services (like EC2, EBS)** to the **edge of 5G networks**
    
- Gives **ultra-low latency** (very fast response) for apps using 5G
    

---

### 🚀 Benefits

- Data **stays within the telecom network** (faster + more secure)
    
- Connects directly to the main AWS Region
    
- No extra cost or special contracts
    

---

### 📌 Use Cases

- Smart Cities
    
- Self-driving or connected vehicles
    
- AR/VR apps, real-time games
    
- Live video streaming
    
- Healthcare (AI diagnostics)

### **AWS Local Zones – Simple Explanation**

- AWS **Local Zones** bring cloud services **closer to users** in specific cities
    
- Helps run **low-latency apps** (fast response) like gaming, video editing, etc.
    

---

### ⚙️ Key Points:

- It's like an **extension of an AWS Region**
    
- You can run **EC2, RDS, EBS, ECS, and more** in these local zones
    
- You can **extend your VPC** to these zones easily
    

---

### 📍 Example:

- Main Region: **N. Virginia (us-east-1)**
    
- Local Zones: **Boston, Chicago, Dallas, Miami**…
    

---

> ✅ **Use Case**: Need AWS power **closer to users** for apps that must respond **very quickly**### **AWS Local Zones – Simple Explanation**

- AWS **Local Zones** bring cloud services **closer to users** in specific cities
    
- Helps run **low-latency apps** (fast response) like gaming, video editing, etc.
    

---

### ⚙️ Key Points:

- It's like an **extension of an AWS Region**
    
- You can run **EC2, RDS, EBS, ECS, and more** in these local zones
    
- You can **extend your VPC** to these zones easily
    

---

### 📍 Example:

- Main Region: **N. Virginia (us-east-1)**
    
- Local Zones: **Boston, Chicago, Dallas, Miami**…
    

---

> ✅ **Use Case**: Need AWS power **closer to users** for apps that must respond **very quickly**

## **Amazon SQS – Standard Queue**

### 🔹 **Definition:**

Amazon Simple Queue Service (SQS) is a **fully managed message queuing service** that lets different parts of your application **communicate asynchronously** by sending and receiving messages via queues.

---

### 🎯 **Purpose:**

To **decouple components** in microservices or distributed systems — meaning, producers and consumers don't have to run at the same time.

---

### ✅ **Key Features:**

|Feature|Description|
|---|---|
|🏗️ **Fully Managed (Serverless)**|No need to manage servers, auto-scales|
|⏱️ **Latency**|<10 ms for sending/receiving|
|📩 **Throughput**|Scales from 1 to thousands of messages/sec|
|🕒 **Message Retention**|Default: 4 days, Max: 14 days|
|♾️ **Unlimited Queue Size**|No hard limit on message count|
|🧹 **Message Deletion**|Deleted automatically after being read|
|📤 **Horizontal Scalability**|Multiple consumers can read in parallel|

---

### 👥 **How It Works:**

- **Producer** sends a message → goes to SQS queue.
    
- **Consumer(s)** poll the queue → process and delete the message.
    
- Messages can be **duplicated** or **delivered out of order** in **Standard Queue** (use FIFO queue to avoid this).

## **Amazon Kinesis Data Streams (KDS)**

### 🔹 **Definition:**

A **fully managed service** for **real-time streaming** of big data — allowing you to **collect, process, and analyze data** continuously and at massive scale.

---

## 🎯 **Use Case:**

> **Kinesis = Real-time Big Data Ingestion**

It’s like a pipeline: data flows in **live**, gets processed **in-memory**, and can be sent to storage or analytics tools **immediately**.

---

## 📦 **How It Works:**

1. **Producers** (e.g., mobile apps, IoT devices, servers) **send data** to a Kinesis stream
    
2. **Shards** in the stream temporarily store the data (default retention: 24 hours, extendable to 7 days)
    
3. **Consumers** (like Lambda, EC2, or Kinesis Data Analytics) **process** the data in real time

## AMAZON SNS(Simple Notification Service)

You want **one message to go to multiple services** (like Shipping, Fraud Detection, Email, etc.)

✅ Example:  
When a **Buying Service** places an order, multiple downstream systems should be notified:

- 📦 Shipping Service
    
- 🛡️ Fraud Detection
    
- 📧 Email Notification
    

---

## 🧩 **Solution: Use Amazon SNS + SQS (Pub/Sub Model)**

### ▶️ **Amazon SNS (Simple Notification Service)**

SNS is used to **publish messages to multiple subscribers** (fan-out model).

---

### 🔧 **How It Works:**

|Role|Function|
|---|---|
|**Publisher**|Buying service sends message to SNS topic|
|**SNS Topic**|Central point that broadcasts the message|
|**Subscribers**|Services like Email, Shipping, Fraud Detection subscribe using their own **SQS queues**, **Lambda functions**, etc.|
|**Delivery**|SNS pushes the message to all subscribed services at once|
## **What is Amazon MQ?**

- Amazon MQ is a **managed message broker service** provided by AWS.
    
- It supports **open-standard messaging protocols**, making it ideal for migrating traditional applications to the cloud.
    

---

## 🔄 **Why Use Amazon MQ?**

- Traditional on-premise apps often use protocols like:
    
    - **MQTT**
        
    - **AMQP**
        
    - **STOMP**
        
    - **OpenWire**
        
    - **WSS**
        
- Rewriting these apps to work with AWS-native services like **SQS** or **SNS** may not be feasible or cost-effective.
    
- **Amazon MQ** acts as a **drop-in replacement** with minimal code changes.
    

---

## 🧪 **Protocol Support:**

Amazon MQ supports industry-standard protocols, allowing **interoperability** with existing systems.

---

## 🧰 **Features of Amazon MQ:**

| Feature                                    | Description                                      |
| ------------------------------------------ | ------------------------------------------------ |
| ✅ **Managed Broker**                       | AWS manages provisioning, patching, and failover |
| 🧩 **Supports Queues**                     | Like Amazon SQS – point-to-point messaging       |
| 📣 **Supports Topics**                     | Like Amazon SNS – publish/subscribe messaging    |
| 🔄 **Multi-AZ Deployment**                 | High availability with automatic failover        |
| ⚙️ **Compatible with ActiveMQ & RabbitMQ** | Two engines supported by Amazon MQ               |
# **Amazon CloudWatch Metrics (Simplified)**

---

### ✅ **What is it?**

- CloudWatch shows **performance data** for AWS services.
    
- A **metric** is just a number to track, like:
    
    - CPU usage
        
    - Network traffic
        
    - Number of requests
        

---

### 🕒 **What makes up a metric?**

(CPUUtilization, NetworkIn
    

---
• Metrics have timestamps
• Can create CloudWatch dashboards of metrics

---

### 🧠 **Example:**

If your EC2 instance CPU goes above 80%, CloudWatch can **alert you** or trigger **Auto Scaling**.

Important Metrics
• EC2 instances: CPU Utilization, Status Checks, Network (not RAM)
• Default metrics every 5 minutes
• Option for Detailed Monitoring (): metrics every 1 minute
• EBS volumes: Disk Read/Writes
• S3 buckets: BucketSizeBytes, NumberOfObjects, AllRequests
• Billing: Total Estimated Charge (only in us-east-1)
• Service Limits: how much you’ve been using a service API
• Custom metrics: push your own metrics


## Amazon CloudWatch Alarms

- Alarms are used to trigger notifications for any metric
- **Actions:**
  - Auto Scaling: increase/decrease EC2 instance count
  - EC2 actions: stop, terminate, reboot, recover
  - SNS: send alert to topic
- Options: sampling, %, max, min, etc.
- Can set time period for evaluation
- Example: billing alarm on AWS bill
- **Alarm States**:
  - OK
  - INSUFFICIENT_DATA
  - ALARM
---

## Amazon EventBridge (formerly CloudWatch Events)

Amazon EventBridge is a service that helps you respond to events happening in AWS or perform actions on a fixed schedule.

There are two main ways to use EventBridge:

Schedule  
-This lets you run tasks at specific times, similar to cron jobs.  
Example: You can run a Lambda function every day at 2 AM.

Event Pattern  
-This allows you to create rules that respond when something happens in AWS.  
Example: When an EC2 instance starts, you can automatically send a message or run a function.

What EventBridge can do:

- Run Lambda functions
    
- Send messages to SQS or SNS
    
- Trigger workflows such as Step Functions
    
- Automate EC2 actions like start, stop, or reboot
  
EventBridge allows you to manage events more efficiently with features like the schema registry, event archiving, and replay.

1. **Schema Registry**  
    You can define and manage the structure (schema) of events. This helps developers understand the format of incoming events.
    
2. **Event Archiving**  
    You can archive events sent to an event bus. Archiving can be applied to all events or filtered ones. The archive can be stored forever or for a specific time period.
    
3. **Event Replay**  
    You can replay archived events at any time. This is useful for testing, recovery, or troubleshooting.

**AWS CloudTrail**

CloudTrail helps you track everything that happens in your AWS account.  
It is useful for **governance** (controlling access), **compliance** (following rules), and **auditing** (checking activity).

CloudTrail is turned on by default — you don’t need to enable it.

It records a history of all actions (called **events**) and **API calls** (commands sent to AWS) done through:

- AWS Console (web dashboard)
    
- SDK (software tools used by developers)
    
- CLI (command-line tool)
    
- AWS services (automatic operations done by AWS)
    

You can send these logs (records) to:

- **CloudWatch Logs** (for monitoring in real time)
    
- **Amazon S3** (for storing the data)
    

You can create a **trail** (tracking setup) for:

- All regions (entire AWS account)
    
- One region (just a specific location)
    

If something like a server or file is deleted, CloudTrail helps you find out **who did it**, **when**, and **how**.

**AWS X-Ray**

AWS X-Ray helps you **find and fix problems** in your application — especially when it’s running live (called **production**).

### The old way of debugging:

- Test your code on your own computer (**locally**).
    
- Add lots of **log statements** (messages that print what the app is doing).
    
- Deploy it again to production to see what went wrong.
    

This process is slow, and every app may use different log styles, so it's hard to read and understand them.

### The problem:

- Debugging is easier when your app is one big block (**monolith**).
    
- But with many small services working together (**distributed system**), it becomes difficult to know what’s happening.
    

Also, there's no **single view** to see how all your services are connected and behaving.


**Advantages of AWS X-Ray** (Simplified)

- Helps find **performance bottlenecks** (slow parts in the system).
    
- Shows how different services are **connected** in microservices (small, independent parts of an app).
    
- Makes it easy to spot **which service is having problems**.
    
- Lets you **see how a user's request travels** through your app.
    
- Helps find **errors and exceptions** (when something breaks or goes wrong).
    
- Checks if you're meeting **SLA** (Service Level Agreement — time limit promised to users).
    
- Shows **where your app is being throttled** (slowed down by AWS limits).
    
- Helps you **identify users** affected by a problem
  
  ### **Amazon CodeGuru – Simple Explanation**

- A **machine learning (ML)** tool that helps you **improve code** and **app performance**
    

---

### 🔧 Two Main Tools:

|Tool|What It Does|
|---|---|
|✅ **CodeGuru Reviewer**|Reviews your code and finds bugs or issues before deployment (development time)|
|📊 **CodeGuru Profiler**|Analyzes running apps and gives tips to improve speed and reduce cost (production time)|

---

> ✅ **Summary**: CodeGuru helps developers **write better code** and make apps **faster and cheaper to run** — using ML.


### **Amazon CodeGuru Profiler – Explained Simply**

- Helps you **see what your app is doing while it runs**
    
- Finds **slow or inefficient parts** of your code (like high CPU or memory use)
    

---

### ⚙️ Key Features:

- 🔧 **Spot and fix bottlenecks** in your code
    
- 🚀 **Improve app performance** and reduce compute cost
    
- 🧠 **Detect anomalies** in runtime behavior
    
- 📦 Gives a **heap summary** (shows which objects use most memory)
    
- ☁️ Works for apps running on **AWS or on-premises**
    
- ✅ Adds **minimal load** to your application
    

---

### 🧪 Example:

> You find that your app uses **too much CPU** just to write logs — Profiler helps you find and fix that.

Here’s a **simple and short explanation** of the **AWS Health Dashboard – Service History**:

---

### 🩺 **AWS Health Dashboard – Service History**

- Shows the **health status** of **all AWS services** across **all regions**
    
- You can view **past issues** by **date**
    
- Includes an **RSS feed** to get real-time updates
    
- Previously known as the **AWS Service Health Dashboard**
    

---

> ✅ **Use it to check:**  
> "Was there a problem with AWS yesterday or today in my region?"

### **AWS Health Dashboard – Your Account**

- **Gives alerts** when AWS issues may **affect your account**
    
- Shows **personalized health info** for your AWS resources
    
- Helps you **respond to issues** and **plan for scheduled maintenance**
    
- Was earlier called **AWS Personal Health Dashboard (PHD)**
    
- Works across your **entire AWS Organization**
    
- It's a **global service**
    

---

### ✅ Key Points:

|Feature|What it Means|
|---|---|
|📢 Alerts|Shows issues that directly impact **you**|
|🔧 Remediation Tips|Helps you fix problems faster|
|📅 Scheduled Info|Know what's coming in advance|
|📊 Organization-wide view|Shows data from all accounts under your org|
### 🌐 **IP Addresses in AWS – Explained Simply**

#### 🧾 **IPv4**

- Older version, **limited to 4.3 billion addresses**
    
- Two types:
    
    - **Public IPv4** – used on the internet
        
        - EC2 gets a new one each time you stop/start it
            
        - **Charged**: $0.005/hour (even Elastic IPs)
            
    - **Private IPv4** – used inside AWS (e.g., 192.168.x.x)
        
        - **Stays the same** even after stop/start
            
- ✅ **Elastic IP** = a fixed public IPv4 that stays the same for your EC2
    

#### 🧾 **IPv6**

- Newer version, **almost unlimited addresses**
    
- All IPv6 addresses in AWS are **public**
    
- Example: `2001:db8:3333:4444:cccc:dddd:eeee:ffff`
    
- ✅ **Free** on AWS
  
  
  ### **VPC & Subnets – Simple Explanation**

- **VPC (Virtual Private Cloud):**  
    Your own **private network** in AWS (covers the whole **region**)
    
- **Subnets:**  
    Divide your VPC into smaller networks (**per Availability Zone**)
    

---

### 🛡️ Types of Subnets

|Subnet Type|Internet Access|Use Case|
|---|---|---|
|🌍 Public|Yes|Web servers, public apps|
|🔒 Private|No|Databases, internal services|

---

### 📍 Routing

- **Route Tables** control how data moves:
    
    - Between subnets
        
    - To/from the internet
        

---

> ✅ **Summary**: VPC = your AWS network; Subnets = slices of that network; Route Tables = traffic rules.

### 🌐 **Internet Gateway (IGW)**

- Lets **public subnet instances** connect **to the internet**
    
- Needed for EC2s with **public IPs**
    
- Must be attached to your **VPC**
    

---

### 🔁 **NAT Gateway (or NAT Instance)**

- Lets **private subnet instances** **access the internet**
    
- But they **stay hidden** from the internet (no incoming traffic allowed)
    
- **NAT Gateway** is managed by AWS (recommended)
    
- **NAT Instance** is self-managed (manual setup)
    

---

### 📌 Use Case Summary:

|Feature|Internet Gateway|NAT Gateway|
|---|---|---|
|Public subnet?|✅ Yes|❌ No|
|Private subnet?|❌ No|✅ Yes|
|Incoming traffic|✅ Allowed|❌ Blocked|
|Outgoing traffic|✅ Allowed|✅ Allowed|

---

> ✅ **Summary**:  
> Use **Internet Gateway** for public access, and **NAT Gateway** to give private instances **internet access without making them public**.

### 🔒 **Network ACL (NACL)**

- Firewall for a **subnet**
    
- Controls **inbound & outbound traffic**
    
- Can **ALLOW or DENY** traffic
    
- Rules use **IP addresses only**
    
- Good for **broad control** over a subnet
    

---

### 🔐 **Security Group**

- Firewall for an **EC2 instance**
    
- Only has **ALLOW** rules (no DENY)
    
- Can use **IP addresses** or **other security groups**
    
- Works at the **instance level**
  
  ### **VPC Flow Logs – Explained Simply**

- **VPC Flow Logs** capture **network traffic data** (who is talking to whom)
    
- Tracks IP traffic for:
    
    - **Entire VPC**
        
    - **Subnets**
        
    - **Network interfaces (ENIs)**
        

---

### 🔍 **Why use it?**

- To **monitor, debug, or troubleshoot**:
    
    - Subnet ↔ Internet
        
    - Subnet ↔ Subnet
        
    - Internet ↔ Subnet
        

---

### 📦 **Where the data goes?**

- You can send flow logs to:
    
    - **S3**
        
    - **CloudWatch Logs**
        
    - **Kinesis Data Firehose**
        

---

> ✅ **Use Case**:  
> Check why an EC2 instance **can’t connect** to another — Flow Logs show the traffic and its status (accepted or rejected).

### **VPC Peering – Explained Simply**

- Used to **connect two VPCs privately** (without going over the internet)
    
- Makes them behave like they're on the **same private network**
    

---

### ⚙️ Key Rules:

- **No overlapping IP ranges** (CIDR blocks)
    
- **Not transitive**:  
    If A is peered with B, and B is peered with C,  
    → **A can't talk to C** unless there's a direct peering
    

---

### 🧠 Example:

> You have one VPC for app servers and one for databases — VPC Peering lets them **communicate securely and privately.**

## VPC Endpoints
• Endpoints allow you to connect to AWS
Services using a private network instead
of the public www network

• This gives you enhanced security and
lower latency to access AWS services
• VPC Endpoint Gateway: S3 &
DynamoDB

• VPC Endpoint Interface: the rest


### **AWS PrivateLink – Explained Simply**

- Lets you **securely share a service** (like an app or API) with **other VPCs**
    
- No need for **VPC Peering**, **Internet Gateway**, or **NAT**
    
- Traffic stays **inside AWS’s private network** — **never goes over the internet**
    

---

### ⚙️ How it Works:

|In Service VPC|In Customer VPC|
|---|---|
|**Network Load Balancer**|**Elastic Network Interface (ENI)** connects to service|

---

### ✅ Use Case:

> A SaaS company can let customers in **other VPCs** use its app **securely** without exposing it publicly.

## • Site to Site VPN
• Connect an on-premises VPN to AWS
• The connection is automatically
encrypted
• Goes over the public internet
## • Direct Connect (DX)
• Establish a physical connection between
on-premises and AWS
• The connection is private, secure and fast
• Goes over a private network
• Takes at least a month to establish

 On-premises: must use a **Customer Gateway (CGW)**
• AWS: must use a **Virtual Private Gateway (VGW)**

## AWS Client VPN
• Connect from your computer using OpenVPN to your private network
in AWS and on-premises

• Allow you to connect to your EC2 instances over a private IP (just as if
you were in the private VPC network)

• Goes over public Internet

### **AWS Transit Gateway (TGW)**

- Acts as a **central hub** to connect **multiple VPCs and on-premises networks**
    
- Supports **transitive routing** (VPC A ↔ TGW ↔ VPC B)
    
- **Solves the problem** of having to create multiple VPC peering links
    
- Follows a **hub-and-spoke (star)** architecture
    

---

### ✅ Key Benefits:

- **Scalable**: connect **1000s of VPCs**
    
- **Simplifies networking**: manage 1 gateway instead of many peering links
    
- **Supports**:
    
    - **Site-to-Site VPN**
        
    - **Direct Connect Gateway**
      

## Amazon Rekognition
• Find objects, people, text, scenes in images and videos using ML

• **Facial analysis** and **facial search** to do user verification, people counting

• Create a database of “familiar faces” or compare against celebrities

• Use cases:
	• Labeling
	• Content Moderation
	• Text Detection
	• Face Detection and Analysis (gender, age range, emotions…)
	• Face Search and Verification
	• Celebrity Recognition
	• Pathing (ex: for sports game analysis)

## Amazon Transcribe

• Automatically convert speech to text

• Uses a deep learning process called automatic speech recognition (ASR) to convert speech to text quickly and accurately

• Automatically remove Personally Identifiable Information (PII) using Redaction (### **PII**?

PII means **any data that can identify a person**, like:

- Name
    
- Email
    
- Phone number
    
- Aadhaar / SSN
    
- Address
    
- Bank details
    

---

### ✂️ What is **Redaction**?

**Redaction** means **hiding or removing** sensitive parts of data before sharing or storing it.

---

### 🛡️ So, PII Redaction =

**Finding and hiding sensitive personal information** (PII) from documents, images, or videos.)

• Supports Automatic Language Identification for multi-lingual audio

• Use cases:
	• transcribe customer service calls
	• automate closed captioning and subtitling
	• generate metadata for media assets to create a fully searchable archive

![[Pasted image 20250727163910.png]]

## Amazon Polly 

• Turn text into lifelike speech using deep learning

• Allowing you to create applications that talk

![[Pasted image 20250727164002.png]]


### **Amazon Translate**

- A **machine learning service** that provides **natural, accurate language translation**
    
- Translates **text from one language to another** in real time or in bulk
    

---

### ✅ **Key Uses**:

- **Localize websites (Localization means **adapting your website** to match the **language, culture, and preferences** of users in different regions.) and apps** for global users
    
- Translate **large volumes of text** (documents, articles, chat, etc.)
    
- Real-time translation for **chatbots, support, emails**
  
  ### 💬 **Amazon Lex**

- A service to **build chatbots and voice bots**
    
- Uses the **same tech as Alexa**
    
- Has two main abilities:
    
    1. **ASR** – Converts **speech to text**
        
    2. **NLU** – Understands the **meaning (intent)** behind the text
        

✅ **Use case**: Build a chatbot that lets users say:

> “I want to check my order status”  
> Lex understands and replies appropriately.

### **Amazon Connect**

A **cloud-based virtual contact center** service.

---

### ✅ **What It Does:**

- Lets companies **receive and make calls**
    
- Build **contact flows** (custom call paths & logic)
    
- No need for physical call center hardware
    

---

### 🔗 **Integrations:**

- Can connect with:
    
    - **CRM systems** (like Salesforce, HubSpot)
        
    - Other **AWS services** (Lex, Lambda, S3, DynamoDB, etc.)

## Amazon Comprehend
• For **Natural Language Processing – NLP**
• Fully managed and serverless service
• Uses machine learning to find insights and relationships in text
	• Language of the text
	• Extracts key phrases, places, people, brands, or events
	• Understands how positive or negative the text is
	• Analyzes text using tokenization and parts of speech
	• Automatically organizes a collection of text files by topic

• Sample use cases:
	• analyze customer interactions (emails) to find what leads to a positive or negative experience
	• Create and groups articles by topics 
	that Comprehend will uncover

### **Amazon SageMaker**

A **fully managed service** that helps **developers and data scientists build, train, and deploy machine learning models** — **without managing servers**.

---

### ✅ **What It Solves:**

- Normally, building ML models needs:
    
    - Writing complex code
        
    - Managing multiple tools
        
    - Provisioning servers
        
- SageMaker does **everything in one place**, automatically.
    

---

### 📚 **Simple Example: Predicting Exam Scores**

Let’s say you want to **predict your exam score** based on:

- How many hours you studied
    
- How much sleep you got
    
- How many past tests you solved
    

🔧 In SageMaker:

1. You upload the data (past exam results).
    
2. SageMaker **trains a model** on it.
    
3. You input new data (study time, sleep).
    
4. SageMaker **predicts your future score**.
    

---

### 🧠 Real Use Cases:

- Fraud detection
    
- Predicting product demand
    
- Analyzing customer behavior
    
- Diagnosing diseases from images

## Amazon Forecast

• Fully managed service that uses ML to deliver highly accurate forecasts
• Example: predict the future sales of a raincoat
• 50% more accurate than looking at the data itself
• Reduce forecasting time from months to hours
• Use cases: Product Demand Planning, Financial Planning, Resource Planning, …


## Amazon Kendra
• Fully managed document search service powered by Machine Learning

• Extract answers from within a document (text, pdf, HTML, PowerPoint, MS Word, FAQs…)

• Natural language search capabilities

• Learn from user interactions/feedback to promote preferred results (Incremental Learning)

• Ability to manually fine-tune search results (importance of data, freshness, custom, …)


### **Amazon Personalize**

A **fully managed machine learning service** that helps you **give real-time personalized recommendations** to users — **just like Amazon.com does**.

---

### ✅ **What It Does:**

- Suggests products, movies, music, or content based on what a user has done before
    
- Helps in **custom marketing** (like emails or SMS with specific offers)
    

---

### 🧺 **Example**:

> A user buys gardening tools →  
> Amazon Personalize suggests:

- Plant seeds
    
- Watering cans
    
- Gardening gloves
    

---

### 🔌 **Where You Can Use It**:

- On your **website or app**
    
- In **email campaigns**
    
- In **SMS promotions**
    

---

### 🧑‍💼 **Use Cases**:

- 🛍️ Retail websites (product suggestions)
    
- 🎬 Media & entertainment (movie/music recommendations)
    
- 📧 Marketing teams (custom email offers)
    

---

### ⚡ **Why Use It**:

- Same tech used by **Amazon.com**
    
- No ML knowledge needed
    
- **Fast setup** – done in **days**, not months
    

Let me know if you'd like a real-world example setup or API use!

### **Amazon Textract**

A fully managed **AI/ML service** that **automatically reads and extracts** text, handwriting, tables, and forms from scanned documents like PDFs and images.

---

### ✅ **What It Does:**

- Reads **typed or handwritten text**
    
- Understands **tables and forms**
    
- Extracts **structured data** — not just plain text
    

---

### 📚 **Examples**:

- Upload a **bank statement PDF** → Textract gives you:
    
    - Account number, date, amount (as structured fields)
        
- Upload a **handwritten doctor’s note** → Textract reads the handwriting
    

---

### 🏥 **Use Cases**:

- 💰 Financial: extract data from **invoices, loan forms**
    
- 🏥 Healthcare: digitize **medical records**, **insurance claims**
    
- 🏛️ Government: process **tax forms**, **passports**, **ID cards**
  
  
  ### **AWS Organizations**

A **global service** (works across all regions) that lets you **manage multiple AWS accounts** (like having separate user accounts for teams or projects) from one place.

---

### 🔑 **Key Concepts**:

- **Master Account** (main admin account): Controls and manages all other accounts
    
- **Member Accounts** (team/project accounts): Managed by the master account
    

---

### 💰 **Cost Benefits**:

- **Consolidated Billing** (single bill for everything): One payment method for all accounts
    
- **Aggregated Usage** (total usage combined): Helps get **volume discounts** (like lower prices when you use more) on services like **EC2, S3**
    
- **Pooling of Reserved EC2 Instances** (sharing pre-booked resources): Save money by sharing reserved instances across accounts
    

---

### 🔒 **Security & Control**:

- **Service Control Policies (SCPs)** (rules at organization level): Restrict what actions or services accounts can use
    
    > Example: Block certain accounts from using expensive resources
    

---

### 🤖 **Automation**:

- **API** (programming interface): You can **automate AWS account creation** (create new accounts using code instead of manually)

### **Multi Account Strategies**

Use **multiple AWS accounts** (instead of just one) for better **organization, security, and cost tracking**.

---

### 🔑 **When to Create Separate Accounts**:

- **Per department** (HR, Marketing, Dev team)
    
- **Per cost center** (to track spend by business unit)
    
- **Dev / Test / Prod** (separate for development, testing, production)
    
- **Regulatory restrictions** (use **SCPs** – Service Control Policies – to enforce rules)
    
- **Resource isolation** (e.g., separate **VPCs** – Virtual Private Clouds – to avoid conflicts)
    
- **Separate service limits** (each AWS account has its own quotas)
    
- **Logging isolation** (dedicated account for logs = better security)
    

---

### 🔄 **Multi Account vs One Account with Multi VPC**:

- Multi-account = **more isolation**, **easier billing**, **stricter controls**
    
- One account with multiple VPCs = **simpler setup**, but **less separation**
    

---

### 🏷️ **Use Tagging Standards**

(Tag AWS resources with names like `project=website` or `env=prod` to track billing)

---

### 📜 **Enable Logging Across All Accounts**:

- **CloudTrail**: Enable in all accounts, **store logs in a central S3 bucket**
    
- **CloudWatch Logs**: Send logs to a **central logging account** for analysis and alerts
  
### **Service Control Policies (SCP)**

SCPs are used to **control what IAM users and roles can or can't do** in AWS accounts **at a higher level** (like a master switch).

---

### 📌 **Key Points**:

- You can **Whitelist** (explicitly allow) or **Blacklist** (explicitly block) IAM actions
    
    > _(Whitelist = allowed only what is listed, Blacklist = deny what is listed)_
    
- Applied at the **OU** (Organizational Unit) or **Account level**
    
    > _(OU = group of AWS accounts inside AWS Organizations)_
    
- SCPs **do not apply to the Master Account** (root-level account in the org)
    
- SCPs apply to **all IAM users and roles in the account**, **including the Root user**
    
- **Service-linked roles** (used by AWS services to function) are **not affected** by SCPs
    
    > _(Example: a role used by CloudWatch to monitor resources can’t be blocked by SCP)_
    
- SCPs require **explicit “Allow”** – nothing is allowed by default
    
    > _(You must manually allow what you want; otherwise, it stays blocked)_
    

---

### 🎯 **Use Cases**:

- **Restrict access to services** (e.g., block use of **EMR** for a certain account)
    
- **Enforce compliance** (e.g., **PCI DSS** by disabling non-compliant services)
  
### **Consolidated Billing** in AWS Organizations

When you **enable consolidated billing**, AWS lets you **group multiple AWS accounts** under one roof for easier billing and better discounts.

---

### ✅ What You Get:

- **Combined Usage**  
    → All accounts **share their resource usage**, so you get **volume-based discounts** (like lower prices for EC2, S3, etc.)  
    → **Reserved Instances** and **Savings Plans** discounts are shared too
    
    > _(This helps reduce overall costs)_
    
- **One Bill**  
    → Get a **single bill** for all accounts instead of managing each one separately
    
    > _(Easier for finance and tracking expenses)_
    
- The **Management Account** (main account) can **disable sharing of Reserved Instances discounts** for specific accounts
    
    > _(Useful if you want certain teams to manage their own costs independently)_
    

### **AWS Control Tower** – Simplified

**What it is:**  
A tool that helps you **quickly and securely set up and manage** multiple AWS accounts using **best practices**.

---

### ✅ **Benefits**:

- **Easy Setup**  
    → Launch your multi-account AWS environment with just a **few clicks**
    
- **Automated Policy Management**  
    → Uses **guardrails** (predefined rules) to **enforce policies automatically**
    
- **Policy Violation Detection**  
    → Alerts you if someone **breaks a rule**, and can even **fix it automatically**
    
- **Compliance Monitoring**  
    → View all your accounts’ **compliance status in a dashboard**
    

---

### 🔄 **How it works**:

- **Built on AWS Organizations**  
    → Control Tower **uses AWS Organizations in the background**  
    → Automatically sets up accounts and applies **SCPs** (Service Control Policies)
    

---

### 🔒 Example Use Case:

You want every new AWS account in your company to **follow security rules automatically**—Control Tower handles this **without manual setup.**

Let me know if you want a diagram or table for this!

### **AWS Resource Access Manager (AWS RAM)** – Simplified

**What it does:**  
Lets you **share AWS resources** with **other AWS accounts** (even across different teams) **without copying or duplicating** them.

---

### ✅ **Why use it?**

- **Share Resources You Own**  
    → You can give access to your **existing resources** to other accounts
    
- **Flexible Sharing**  
    → Share with **any AWS account** or accounts **within your AWS Organization**
    
- **Avoid Duplication**  
    → No need to **create separate copies** of the same resource for each account  
    → Saves **cost and effort**
    

---

### 📦 **Supported Resources Include:**

- **Aurora (database clusters)**
    
- **VPC Subnets (network sections)**
    
- **Transit Gateway (for network routing)**
    
- **Route 53 (DNS service)**
    
- **EC2 Dedicated Hosts**
    
- **License Manager Configurations**
    

---

### 💡 Example:

You have a **Transit Gateway** in Account A and want Account B to use it.  
Instead of creating a new one in B, **just share it using AWS RAM**.

Let me know if you want a visual summary or table too

### **AWS Service Catalog** – Simplified

**What it is:**  
A **self-service portal** that lets users **launch only approved AWS resources** (like VMs, databases, etc.)—**pre-configured by admins**.

---

### ✅ **Why it's useful:**

- **Too Many Options?**  
    → New users may feel **overwhelmed** and create setups that are **not secure or compliant**
    
- **Standardized Setup**  
    → Admins can create and share **pre-approved templates** for things like EC2, RDS, S3...
    
- **Safe & Fast Deployment**  
    → Users can **quickly launch** resources through a **ready-made catalog** without worrying about settings
    
- **Compliance Maintained**  
    → Ensures everyone follows the **same rules, configurations, and policies**
    

---

### 💡 Example:

Instead of every developer manually setting up a server, you give them a **"Launch Server" button** that starts an EC2 instance **with the right settings, permissions, and tags**—via **AWS Service Catalog**.

Let me know if you want a real-life use case or visual summary too!

## Pricing Models in AWS
• AWS has 4 pricing models:
• **Pay as you go:** pay for what you use, remain agile, responsive, meet scale
demands
**• Save when you reserve:** minimize risks, predictably manage budgets,
comply with long-terms requirements
• Reservations are available for EC2 Reserved Instances, DynamoDB Reserved
Capacity, ElastiCache Reserved Nodes, RDS Reserved Instance, Redshift Reserved
Nodes
**• Pay less by using more:** volume-based discounts
**• Pay less as AWS grow**

![[Pasted image 20250727195407.png]]

## Compute Pricing-EC2

• Only charged for what you use • Number of instances • Instance configuration:
• Physical capacity 
• Region
• OS and software
• Instance type 
• Instance size
• ELB running time and amount of data processed
• Detailed monitoring

![[Pasted image 20250727195646.png]]

Compute Pricing
– Lambda & ECS
• Lambda: 
	• Pay per call
	• Pay per duration

• ECS:
	• EC2 Launch Type Model: No additional fees, you pay for
	AWS resources stored and created in your application

• Fargate
	• Fargate Launch Type Model: Pay for vCPU and memory resources allocated to your applications in your containers
	

 ### **Amazon S3 Pricing – Simplified**

You are charged based on:

---

#### 🗂️ 1. **Storage Class**

- **S3 Standard** – for frequent access
    
- **S3 Infrequent Access (IA)** – for occasional use
    
- **S3 One-Zone IA** – cheaper, stored in one zone
    
- **S3 Intelligent Tiering** – automatically moves data to cheaper storage
    
- **S3 Glacier / Glacier Deep Archive** – for backups and long-term storage
    

_(Each class has a different price depending on speed and durability)_

---

#### 📦 2. **Number and Size of Files**

- More files or bigger files = more cost
    
- Discounts available as your total usage increases _(tiered pricing)_
    

---

#### 📥 3. **Request Types**

- You pay for each **GET**, **PUT**, **DELETE**, etc.
    
- Price depends on request **type and frequency**
    

---

#### 🌐 4. **Data Transfer Out**

- You are charged for data that leaves the **S3 Region**  
    _(e.g., when someone downloads your file from a different location)_
    

---

#### ⚡ 5. **S3 Transfer Acceleration**

- Faster uploads/downloads using **Edge Locations**
    
- Extra cost for this feature
    

---

#### 🔄 6. **Lifecycle Transitions**

- Moving data automatically between storage classes (e.g., from Standard to Glacier)
    
- You pay for **transition operations**
    

---

### 🔁 Similar Service: **EFS**

- Also **pay-per-use**
    
- Has **infrequent access** & **lifecycle rules**, just like S3
    
- Used more with **EC2 file systems** than object storage
  
  ### **Storage Pricing – EBS**

- **Volume type** (type of EBS like SSD or Magnetic, based on performance)
    
- **Storage volume in GB per month provisioned** (you pay for the size you allocate monthly)
    
- **IOPS**:
    
    - **General Purpose SSD**: Included (IOPS cost is already in the price)
        
    - **Provisioned IOPS SSD**: Provisioned amount in IOPS (you pay for the IOPS you set)
        
    - **Magnetic**: Number of requests (you pay based on how many read/write operations happen)
        
- **Snapshots**:
    
    - Added data cost per GB per month (charged for how much backup data is stored monthly)
        
- **Data transfer**:
    
    - **Outbound data transfer** are tiered for volume discounts (more data out = lower per GB price)
        
    - **Inbound is free** (no cost to bring data into EBS)

### 🔸 **Database Pricing – RDS**

- **Per hour billing**  
    _(You are charged every hour the database runs)_
    
- **Database characteristics**:
    
    - **Engine** _(e.g., MySQL, PostgreSQL – the type of DB)_
        
    - **Size** _(storage space in GB)_
        
    - **Memory class** _(instance type based on RAM/CPU)_
        
- **Purchase type**:
    
    - **On-demand** _(pay per hour, no commitment)_
        
    - **Reserved instances (1 or 3 years) with required up-front**  
        _(commitment plan with lower cost, needs partial/full upfront payment)_
        
- **Backup Storage**:
    
    - _No additional charge for backup storage up to_ **100% of your total database storage** _in a region_  
        _(free backup space equal to your DB size; beyond that, it's chargeable)_
        

---

Let me know if you want the same format for DynamoDB, Redshift, or others!

- **Additional storage (per GB per month)**  
    _(Extra storage is charged monthly, based on how many GB you use)_
    
- **Number of input and output requests per month**  
    _(The more read/write operations, the higher the cost)_
    
- **Deployment type (storage and I/O are variable)**:
    
    - **Single AZ** _(DB deployed in one availability zone — lower cost)_
        
    - **Multiple AZs** _(DB replicated across zones — higher cost for better availability)_
        
- **Data transfer**:
    
    - **Outbound data transfer are tiered for volume discounts**  
        _(Cost reduces as the amount of data sent out increases)_
        
    - **Inbound is free**  
        _(No cost for data coming into RDS)_

## Content Delivery– CloudFront
• Pricing is different across different geographic regions
• Aggregated for each edge location, then applied to your bill 
• Data Transfer Out (volume discount)
• Number of HTTP/HTTPS requests

### 🔸 **Savings Plan**

- **Commit a certain $ amount per hour** for **1 or 3 years**  
    _(You agree to spend a fixed amount per hour for a long time to get discounts)_
    
- **Easiest way** to setup **long-term commitments** on AWS  
    _(Simple option to save money if you're using AWS regularly)_
    

---

#### ✅ **EC2 Savings Plan**

- **Up to 72% discount** compared to **On-Demand**  
    _(Biggest savings for regular EC2 users)_
    
- **Commit to usage** of **specific instance families** (e.g. C5, M5) in a **region**  
    _(Locked to instance type and location)_
    
- Flexible across:  
    • **Availability Zones (AZs)**  
    • **Sizes** (e.g., m5.xlarge to m5.4xlarge)  
    • **Operating Systems (OS)**: Linux / Windows  
    • **Tenancy** (shared or dedicated host)
    
- Payment options:  
    • **All upfront**, **partial upfront**, or **no upfront**
    

---

#### ✅ **Compute Savings Plan**

- **Up to 66% discount** compared to **On-Demand**  
    _(Slightly less discount but much more flexible)_
    
- Works across:  
    • **Any family**, **region**, **size**, **OS**, **tenancy**  
    • **Compute options**: **EC2**, **Fargate**, **Lambda**
    

---

#### ✅ **Machine Learning Savings Plan**

- Applies to **SageMaker** workloads
    

---

- Setup from: **AWS Cost Explorer console**
    
- Pricing estimates: [aws.amazon.com/savingsplans/pricing](https://aws.amazon.com/savingsplans/pricing)
    

---

Let me know if you want **Reserved Instances vs Savings Plans** comparison next.

## AWS Compute Optimizer
• Reduce costs and improve performance by recommending optimal AWS resources for your
workloads
• Helps you choose optimal configurations and right
- size your workloads (over/under provisioned)
• Uses Machine Learning to analyze your resources’
configurations and their utilization CloudWatch
metrics
	• Supported resources 
	• EC2 instances 
	• EC2 Auto Scaling Groups
	• EBS volumes
	• Lambda functions
	• Lower your costs by up to 25%
	• Recommendations can be exported to S3

Billing and Costing Tools
💰 Estimating costs in the cloud (To know in advance how much you’ll spend)
Pricing Calculator (Tool to estimate monthly AWS bill)

📊 Tracking costs in the cloud (To see where money is going)
Billing Dashboard (Shows current usage and charges)

Cost Allocation Tags (Track costs by project, team, or purpose using tags)

Cost and Usage Reports (Detailed billing data in CSV format)

Cost Explorer (Visual tool to analyze spending over time)

🔔 Monitoring against cost plans (To control spending and stay on budget)
Billing Alarms (Alerts if charges cross a limit — set in CloudWatch)

Budgets (Set limits and get alerts when close to the budget)

### **Cost Allocation Tags**

- **Use tags to track AWS costs** _(helps break down and analyze spending in detail)_
    

#### ✅ **Types of Tags:**

1. **AWS Generated Tags**
    
    - **Auto-applied** to the resource when it's created
        
    - Start with prefix `aws:` (example: `aws:createdBy`)
        
    - _(Helps track who/what created the resource)_
        
2. **User-defined Tags**
    
    - **Created by you**
        
    - Start with prefix `user:`
        
    - _(Used for custom labeling like user:project, user:team, user:env)_

### **Tagging and Resource Groups**

- **Tags = Labels** (key-value pairs) used to **organize AWS resources**
    

#### 🔖 **Where tags are used:**

- **EC2** (instances, images, load balancers, security groups)
    
- **RDS**, **VPC**, **Route 53**, **IAM users**, etc.
    
- **CloudFormation**: tags are **auto-applied to all resources it creates**
    

#### 📝 **Common Tags:**

- `Name`, `Environment` (dev, prod), `Team`, etc.
    

#### 📦 **Resource Groups:**

- Group resources that share **same tags**
    
- Helps **view/manage** them together
    

#### 🧰 **Tag Editor:**

- Tool to **create, edit, and manage tags** across AWS resources

### **AWS Cost and Usage Reports (CUR)**

- Gives **detailed breakdown** of your AWS **costs and usage**
    
- Most **comprehensive** AWS billing report
    

#### 📄 What it includes:

- **Usage per service** (like EC2, S3, etc.)
    
- **IAM users' usage**
    
- **Hourly or daily** entries
    
- **Tags** (if cost allocation tags are enabled)
    
- Info on **pricing**, **reservations**, e.g., **EC2 Reserved Instances (RIs)**
    

#### 🔗 **Integrations:**

- Can be analyzed using:
    
    - **Athena** (query with SQL)
        
    - **Redshift** (data warehouse)
        
    - **QuickSight** (dashboards & visuals)

### **Cost Explorer** (Tool for analyzing AWS spending)

- **Visualize** (see graphs) and **understand** AWS **costs & usage**
    
- Create **custom reports** (your own views of data)
    
- View data by:
    
    - **Total costs** (across all accounts)
        
    - **Monthly**, **hourly**, or **per-resource** detail (granularity)
        
- Helps choose the **best Savings Plan** (to save money)
    
- **Forecast usage** for up to **12 months** (based on past data)

![[Pasted image 20250728014205.png]]

### **AWS Budgets** (Tool to **track** AWS spending & usage)

- Set a **budget** → Get **alerts** if you go over it
    
- Supports **4 budget types**:
    
    - **Usage Budget** (how much you're using)
        
    - **Cost Budget** (how much you're spending)
        
    - **Reservation Budget** (track Reserved Instances)
    
    - **Savings Plans Budget**
        
- For **Reserved Instances**:
    
    - Track **usage** and **utilization**
        
    - Works with **EC2, ElastiCache, RDS, Redshift**
        
- Up to **5 alerts** via **SNS notifications** per budget
    
- You can **filter** by:
    
    - **Service, Account, Tag, Instance Type, Region**, etc.
        
- First **2 budgets are free**, then **$0.02/day** for each extra

### 📈 **AWS Cost Anomaly Detection**

(Auto-detect unusual AWS spending using **Machine Learning**)

- **Monitors cost & usage** in real-time
    
- **Learns your normal spending pattern**
    
- **No need to set thresholds** manually
    
- Detects:
    
    - **Sudden spikes** in cost
        
    - **Gradual increases** in spending
        
- Can monitor by:
    
    - **AWS service**
        
    - **Account**
        
    - **Tags**
        
    - **Cost categories**
        
- Sends alerts via **SNS** (immediate or daily/weekly summary)
    
- Includes a **report with root cause analysis** (why it happened)

### **AWS Service Quotas**

(Limits on how much AWS resource you can use)

- **Notify** you when you're **close to a quota limit**  
    – e.g., _Lambda concurrent executions limit_
    
- You can **create CloudWatch alarms** to monitor usage
    
- Options if you're near the limit:
    
    - **Request an increase** from AWS
        
    - **Shutdown or reduce usage** before hitting the limit
### **AWS Trusted Advisor**

(High-level tool to assess and improve your AWS account – no setup needed)

- **Analyzes your AWS account** and gives **recommendations** in **6 categories**:
    
    1. **Cost Optimization** (Save money)
        
    2. **Performance** (Run faster)
        
    3. **Security** (Stay protected)
        
    4. **Fault Tolerance** (Avoid downtime)
        
    5. **Service Limits** (Avoid hitting AWS limits)
        
    6. **Operational Excellence** (Best practices)
        
- **Business & Enterprise Support Plans**:  
    – Get **full set of checks**
    
- **Programmatic Access**:  
    – Use **AWS Support API** to access results via code

### **AWS Basic Support Plan** (Free starter support for all accounts)

- **Customer Service & Communities**  
    → 24x7 access to **help**, **guides**, **whitepapers**, and **forums** (learn and ask anytime)
    
- **AWS Trusted Advisor**  
    → Access to **7 core checks** (like cost, security, performance) to follow AWS **best practices**
    
- **AWS Personal Health Dashboard**  
    → **Personalized alerts** when your **AWS services face issues** (helps monitor your resources)

### **AWS Developer Support Plan**

Includes **everything in Basic Plan**, plus:

- **Business hours email access** to **Cloud Support Associates** (email help from AWS experts during working hours)
    
- **Unlimited cases / contacts** (you can raise as many issues as needed)
    
- **Response times** based on issue type:
    
    - **General guidance** (non-urgent help): within **24 business hours**
        
    - **System impaired** (some features not working): within **12 business hours**
### 🏢 **AWS Business Support Plan (24/7)**

✅ **For production workloads** (live apps, real users)

#### Features:

- **Trusted Advisor – Full checks** + **API access** (automated help for cost, performance, security)
    
- **24x7 support** via **phone, email, and chat** with AWS Cloud Support Engineers
    
- **Unlimited cases and contacts** (your whole team can raise unlimited support tickets)
    
- **Infrastructure Event Management** (extra fee) – AWS helps with planning big events like product launches
    

#### 🚨 Case Severity & Response Times:

| Issue Type                 | Response Time       |
| -------------------------- | ------------------- |
| General guidance           | < 24 business hours |
| System impaired            | < 12 business hours |
| Production system impaired | < 4 hours           |
| Production system down     | < 1 hour            |
### 🏢 **AWS Enterprise On-Ramp Support Plan**

✅ **Best for production or business-critical workloads**

#### 🚀 Includes Everything from Business Plan **plus**:

- 👨‍💼 **Technical Account Managers (TAMs)** – expert advisors to guide your cloud journey
    
- 💬 **Concierge Support Team** – help with **billing** and **account questions**
    
- 🛠️ **Infrastructure Event Management** – help during big launches or migrations
    
- 📋 **Well-Architected & Ops Reviews** – AWS checks if you're following best practices
    

---

#### 🚨 Faster Response Times:

| Issue Type                 | Response Time    |
| -------------------------- | ---------------- |
| Production system impaired | < 4 hours        |
| Production system down     | < 1 hour         |
| **Business-critical down** | **< 30 minutes** |
### 🏢 **AWS Enterprise Support Plan**

✅ **Best for mission-critical workloads**

#### 💼 Includes everything from Business Support Plan **plus**:

- 👤 **Dedicated Technical Account Manager (TAM)** – your personal AWS expert
    
- 💬 **Concierge Support Team** – help with **billing** and **accounts**
    
- 🛠️ **Infrastructure Event Management** – for smooth big launches
    
- 🧠 **Well-Architected & Operations Reviews** – AWS reviews your setup
    
- 🚨 **AWS Incident Detection and Response** (for extra fee) – AWS watches for issues and responds
    

---

#### 🚨 Fastest Response Times:

| Issue Type                 | Response Time    |
| -------------------------- | ---------------- |
| Production system impaired | < 4 hours        |
| Production system down     | < 1 hour         |
| **Business-critical down** | **< 15 minutes** |
### **AWS STS (Security Token Service)**

- **Creates temporary credentials** (short-term, limited-access) to access AWS resources
    
- **You set expiration time** (like 15 min to a few hours)
    

---

### ✅ **Use Cases**

1. **Identity Federation**
    
    - Users from **external systems** (like Google or Active Directory)
        
    - Get **STS tokens** to access AWS **without creating IAM users**
        
2. **IAM Roles (Cross/Same Account Access)**
    
    - Allows **Account A to access Account B** securely using temporary roles
        
3. **IAM Roles for EC2**
    
    - EC2 instance gets **temporary access** to AWS services (like S3)
### **Amazon Cognito (Simplified)**

- **Manages users** for your **Web and Mobile apps**  
    (can handle **millions of users**)
    
- Instead of giving **IAM users** to each person (which is for AWS access),  
    you create **Cognito users** – safer and easier to manage.
    
### 🖥️ Microsoft Active Directory (AD) – Simplified

- **What is it?**  
    A **user management system** found on **Windows Servers** (with AD Domain Services).
    
- **What does it store?**  
    A **database of objects** like:
    
    - User Accounts
        
    - Computers
        
    - Printers
        
    - File Shares
        
    - Security Groups
        
- **Why use it?**  
    For **centralized security management** —  
    You can **create accounts** and **assign permissions** in one place for the entire organization.

    

📌 Example: In a company, AD controls **who can log into which computer**, **who can print**, and **who can access files**.
---

### ✅ **Why use it?**

- Sign-up, sign-in, and password reset features built-in
    
- Supports **Google, Facebook, Apple login** (OAuth providers)
    
- Easy user authentication and authorization for apps

### 🗂️ AWS Directory Services – Simplified

1. **AWS Managed Microsoft AD**
    
    - A **fully managed Active Directory** on AWS.
        
    - You can **create and manage users** directly in AWS.
        
    - **Supports MFA** (multi-factor authentication).
        
    - Can **connect ("trust") with your on-premise AD** system.
        
2. **AD Connector**
    
    - A **proxy (gateway)** that connects AWS to your **on-premise AD**.
        
    - **Users are still managed on-premises** (not in AWS).
        
    - Also **supports MFA**.
        
3. **Simple AD**
    
    - A **lightweight, AD-compatible directory** managed on AWS.
        
    - Good for **basic use** (small teams, dev/test environments).
        
    - **Can’t connect to your on-premise AD**.
        

📌 **Summary**:

- Use **Managed AD** if you want full AD in the cloud.
    
- Use **AD Connector** if you already have AD on-prem.
    
- Use **Simple AD** for basic needs without on-premise integration.

### ✅ AWS IAM Identity Center (Simplified)

> **Previously called** AWS Single Sign-On (SSO)

---

🔐 **What It Does:**

- Lets users **log in once** and access:
    
    - ✅ All **AWS accounts** in your **AWS Organization**
        
    - ✅ Business apps like **Salesforce**, **Box**, **Microsoft 365**
        
    - ✅ Any **SAML 2.0-enabled apps**
        
    - ✅ **EC2 Windows instances**
        

---

👥 **Where It Gets User Info From (Identity Sources):**

- 🔸 **Built-in** identity store (within IAM Identity Center)
    
- 🔸 **External providers** like:
    
    - **Active Directory (AD)**
        
    - **Okta**
        
    - **OneLogin**
        
    - and more…
        

---

📌 **Use Case Example:**  
You’re an employee → You log in once → You can access your AWS console + Salesforce + Microsoft 365 without logging in again.


## ✅ **Amazon WorkSpaces (Simplified)**  
_Managed Virtual Desktop Service (DaaS)_

---

💻 **What It Does:**  
Provides virtual **Windows or Linux desktops** in the cloud — no need to manage physical desktops or VDI systems.

---

📌 **Key Features:**

- 🧑‍💻 **Managed DaaS (Desktop as a Service)**
    
- ⚡ **Fast and scalable** — can launch desktops for thousands of users
    
- 🔐 **Secure** — integrates with AWS KMS for encryption
    
- 💰 **Pay-as-you-go** — billed **monthly or hourly**
    

---

📌 **Use Case Example:**  
Company wants to give remote employees secure Windows desktops without buying physical laptops — use WorkSpaces instead.

Let me know if you want a comparison with AppStream 2.0!

### **Amazon AppStream 2.0 (Simplified)**  
_Stream Desktop Apps to Any Computer via Browser_

---

💡 **What It Does:**  
Lets you **stream desktop applications** (like AutoCAD, Photoshop, Excel) to users via a **web browser** — no need to install apps or set up servers.

---

📌 **Key Features:**

- 🚀 **Desktop application streaming** (not full desktops like WorkSpaces)
    
- 🌐 **Access apps through any browser** — no installation needed
    
- 🖥️ **No infrastructure to manage**
    
- 💰 **Pay-as-you-go** pricing
    

---

📌 **Use Case Example:**  
A design school wants students to use licensed Adobe software from home → AppStream 2.0 streams the app through the browser — students use it like it's on their PC.

---

### 🔷 **Amazon WorkSpaces**

- **What it is:** A **fully managed virtual desktop** (VDI) service.
    
- **User experience:** Users **log into a Windows/Linux desktop**, just like a physical PC.
    
- **Use case:** Best when you need a **full desktop environment**.
    
- **Apps:** Can run **native apps or WAM apps** (Windows App Migration).
    
- **Access type:** Always-on or on-demand VDI.
    
- **Example:** Corporate employees who need access to a secured Windows desktop from any device.
    

---

### 🔷 **Amazon AppStream 2.0**

- **What it is:** A **desktop application streaming** service.
    
- **User experience:** Stream **specific apps** through a **web browser**—no desktop/VDI.
    
- **Use case:** Best when users only need **one or a few apps**, not the whole desktop.
    
- **Apps:** Only stream the application, not the full OS.
    
- **Access type:** On-demand, web browser-based.
    
- **Instance flexibility:** You can configure **specific resources (CPU, GPU, RAM)** for each app.
    
- **Example:** Students accessing a CAD software remotely through a browser.
  
### 🔷 **AWS IoT Core**

- **IoT (Internet of Things):** Refers to **smart devices** (like sensors, smart bulbs, or industrial machines) that are connected to the internet and **share data**.
    

---

### ✅ **Key Features of AWS IoT Core**

|Feature|Description|
|---|---|
|🔌 **Device Connectivity**|Easily connect millions (or billions) of devices to AWS Cloud.|
|☁️ **Serverless**|No need to manage infrastructure – AWS handles it.|
|🔐 **Secure**|Uses encryption and authentication to keep device data safe.|
|📶 **Offline Communication**|Apps can still send messages to devices **even if they are offline**. They get them when they reconnect.|
|🔗 **Integrates with AWS**|Works well with services like **Lambda (for automation), S3 (for storage), SageMaker (for ML), DynamoDB**, etc.|
|📊 **Real-Time Data**|Collect, process, analyze, and respond to data in real time.|

---

### 🧠 Real-Life Example:

Imagine you have a **smart irrigation system** on a farm.

- The sensors detect **soil moisture**, **weather**, and **water flow**.
    
- Data is sent to **AWS IoT Core**, then passed to **Lambda** (to make decisions), **S3** (for storage), and **SageMaker** (to predict drought risk).
    
- Even if a device goes offline, AWS IoT Core stores and delivers messages later.

### 🎬 **Amazon Elastic Transcoder**

- A **media conversion service**.
    
- It **converts (transcodes)** videos or audio files stored in **Amazon S3** into formats that work on **phones, tablets, laptops**, etc.
    

---

### ✅ **Key Features**

|Feature|Description|
|---|---|
|🔄 **Media Conversion**|Changes video/audio formats so they play properly on different devices (e.g., iPhone, Android, TV).|
|☁️ **S3 Integration**|Works directly with files stored in Amazon S3.|
|📈 **Scalable**|Can process thousands of videos, no matter the size.|
|💸 **Cost-Effective**|Pay only for the **duration** of the media transcoded.|
|🔐 **Fully Managed & Secure**|No server setup required; AWS handles everything securely.|
|🧑‍💻 **Easy to Use**|Simple to set up using presets for common formats and bitrates.|

---

### 📱 Real-Life Example:

You have a video tutorial saved in S3 in **.mov** format. You want to share it on your app, and users might use Android, iOS, or web.  
**Elastic Transcoder** converts it into formats like **.mp4** (for web and Android) and **.m4v** (for iOS), ensuring smooth playback on all devices.

### 🔷 **AWS AppSync**

- A **serverless service** to **store, sync, and update data in real time** across **web and mobile apps**.
    
- It uses **GraphQL**, a modern API technology developed by Facebook.
    

---

### ✅ **Key Features**

|Feature|Description|
|---|---|
|⚡ **Real-Time Sync**|Automatically keeps data updated across all devices.|
|🔁 **Offline Support**|Users can work offline, and changes sync when they reconnect (replaces Cognito Sync).|
|🔧 **GraphQL API**|Query exactly the data you need (unlike REST which gives everything).|
|🤖 **Auto Code Generation**|AppSync can generate frontend code automatically.|
|🔗 **Easy Integration**|Connects easily to **DynamoDB**, **Lambda**, **Aurora**, etc.|
|🔐 **Fine-Grained Security**|Use IAM, Cognito, or API keys to control access to specific data or fields.|
|🛠️ **Works with AWS Amplify**|Amplify can auto-configure AppSync behind the scenes for mobile/web apps.|

---

### 📱 Real-Life Example:

Imagine building a **chat app**.

- Messages are sent from one device → **AppSync** → stored in **DynamoDB**.
    
- All connected users see messages **instantly (real-time)**.
    
- If someone is offline, the chat syncs **automatically when they reconnect**.

### 🔷 **AWS Amplify**

- A **complete toolkit** for building and deploying **scalable full-stack** **web and mobile apps**—fast and easy.
    
- Helps **frontend developers** add backend features without deep cloud knowledge.
    

---

### ✅ **Key Features**

| Feature                        | Description                                                                      |
| ------------------------------ | -------------------------------------------------------------------------------- |
| 🔐 **Authentication**          | Easily add user sign-up, login, and multi-factor auth (uses Amazon Cognito).     |
| 💾 **Storage**                 | Store user files like images/videos (uses Amazon S3).                            |
| 🔗 **API (REST & GraphQL)**    | Connect your app to backends (via API Gateway / AppSync + Lambda).               |
| 🚀 **CI/CD**                   | Automatically build and deploy from GitHub, GitLab, Bitbucket, etc.              |
| 📡 **PubSub (Real-Time Data)** | Real-time features like chat or live dashboards.                                 |
| 📊 **Analytics**               | Track user behavior and app usage.                                               |
| 🤖 **AI/ML**                   | Add ML features like text translation, image recognition, or sentiment analysis. |
| 🧪 **Monitoring**              | View performance, logs, and errors.                                              |
### 🧱 **AWS Infrastructure Composer**

- A **visual tool** to help you **design, build, and deploy serverless applications** on AWS—**without deep AWS expertise**.
    

---

### ✅ **Key Features**

|Feature|Description|
|---|---|
|🖼️ **Drag-and-Drop UI**|Visually create and connect AWS resources (like Lambda, API Gateway, S3, etc.).|
|🧠 **Beginner-Friendly**|No need to write CloudFormation manually—just design it visually.|
|⚙️ **Auto IaC Generation**|Automatically creates **Infrastructure as Code** using **AWS CloudFormation** behind the scenes.|
|🔁 **Import Existing Templates**|You can **import CloudFormation or SAM templates** to see them as diagrams and edit them.|
|🔗 **Resource Interaction**|Configure how AWS services interact (e.g., S3 → Lambda → DynamoDB).|
|🚀 **Deploy Directly**|You can **deploy infrastructure** from the tool itself.|

---

### 🧑‍💻 Real-Life Example:

You're building a serverless app where:

- A user uploads a file to **S3**
    
- This triggers a **Lambda function**
    
- The Lambda writes data to **DynamoDB**
    

With **Infrastructure Composer**, you can **drag and connect** these resources visually, and it will:

- Auto-generate the CloudFormation code
    
- Let you deploy everything with a click

## AWS Device Farm
• Fully-managed service that tests your web and mobile apps against
desktop browsers, real mobile devices, and tablets

• Run tests concurrently on multiple devices (speed up execution)

• Ability to configure device settings (GPS, language, Wi-Fi, Bluetooth, …)

## AWS Backup
• Fully-managed service to centrally manage and automate backups across
AWS services
• On-demand and scheduled backups
• Supports PITR (Point-in-time Recovery)
• Retention Periods, Lifecycle Management, Backup Policies, …
• Cross-Region Backup
• Cross-Account Backup (using AWS Organizations)

### 🔄 **AWS Elastic Disaster Recovery (DRS)**

- A **disaster recovery service** that helps you **recover your servers (physical, virtual, or cloud-based)** into AWS **quickly and easily**.
    
- Previously known as **CloudEndure Disaster Recovery**.
    

---

### ✅ **Key Features**

|Feature|Description|
|---|---|
|🚨 **Disaster Recovery**|Recover from failures, ransomware, or disasters by spinning up replicas in AWS.|
|💽 **Continuous Replication**|Real-time **block-level data replication**, not just backups—so your recovery is always up to date.|
|🧱 **Broad Compatibility**|Works with **databases** (like Oracle, MySQL, SQL Server), **enterprise apps** (like SAP), and almost any OS.|
|🚀 **Fast Recovery**|You can launch your servers in AWS **within minutes** after failure.|
|💰 **Cost-Efficient**|Keeps lightweight replicas running (low-cost), and spins up full environments only when needed.|
|🛡️ **Ransomware Protection**|If your data is compromised on-prem, you can launch a clean version on AWS quickly.|

---

### 🏥 Real-Life Example:

A hospital runs critical systems on local servers.

- They use **AWS DRS** to continuously replicate data to AWS.
    
- If ransomware hits or servers crash, they can **instantly boot up replicas** in AWS and keep running.

## AWS DataSync
• Move large amount of data from on-premises to AWS
• Can synchronize to: Amazon S3 (any storage classes – including
Glacier), Amazon EFS, Amazon FSx for Windows
• Replication tasks can be scheduled hourly, daily, weekly
• The replication tasks are incremental after the first full load

## Cloud Migration Strategies: The 7Rs
### 🧠 **1. Retire**

- **What:** Turn off unused or unnecessary applications.
    
- **Why:** Save cost (10–20%) and reduce attack surface.
    
- **Example:** An old report-generating tool no one uses anymore is **shut down** before migrating.
    

---

### 🧠 **2. Retain**

- **What:** **Keep as is** — don’t migrate.
    
- **Why:** Security, compliance, legacy dependencies, or no cloud benefit.
    
- **Example:** A **mainframe application** with no cloud equivalent is left on-prem for now.
    

---

### 🧠 **3. Relocate**

- **What:** Move workloads **without changing them**, even across AWS accounts/regions.
    
- **Why:** Helpful for **VMware or SSDC setups** moving to **VMware Cloud on AWS**.
    
- **Example:** Moving VMs from a **data center to AWS** without changing app structure.
    

---

### 🧠 **4. Rehost ("Lift and Shift")**

- **What:** Move applications "as-is" to AWS.
    
- **Why:** Fastest method, no code changes. Can save ~30% on costs.
    
- **Example:** Using **AWS Application Migration Service** to shift a database server from on-prem to EC2.
    

---

### 🧠 **5. Replatform ("Lift and Reshape")**

- **What:** Minor tweaks to use cloud services.
    
- **Why:** Gain **performance and cost benefits** without re-architecting.
    
- **Example:** Move your app from an on-prem server to **Elastic Beanstalk**, or your DB to **Amazon RDS**.
    

---

### 🧠 **6. Repurchase ("Drop and Shop")**

- **What:** Replace with a **SaaS solution**.
    
- **Why:** Save maintenance and get faster deployment.
    
- **Example:** Replace your old HR system with **Workday**, or your CRM with **Salesforce**.
    

---

### 🧠 **7. Refactor / Re-architect**

- **What:** Completely **redesign** the application for the cloud.
    
- **Why:** When you need scalability, agility, better performance.
    
- **Example:** Break a **monolith** into **microservices**, and use **Lambda + S3 + API Gateway** for a serverless setup.
    

---

### 📌 Summary Table:

|Strategy|Quick Description|Real-Life Example|
|---|---|---|
|**Retire**|Turn off unused apps|Deleting an old internal survey app|
|**Retain**|Keep on-prem|Legacy mainframe with tight compliance|
|**Relocate**|Move VMs to AWS as-is|VMware SSDC → VMware Cloud on AWS|
|**Rehost**|Lift and shift|On-prem server → EC2|
|**Replatform**|Minor cloud tweaks|DB to RDS, App to Beanstalk|
|**Repurchase**|Move to SaaS|Internal CRM → Salesforce|
|**Refactor**|Total redesign using cloud-native|Monolith → Microservices with Lambda|

---

Let me know if you'd like a diagram or cheat sheet!

### **AWS Application Discovery Service**

Helps you **plan cloud migrations** by collecting detailed info about your **on-premise infrastructure** (servers, usage, dependencies).

---

### ✅ **Why Use It?**

Before migrating to AWS, you need to know:

- What servers you have
    
- What’s running on them
    
- How much they’re being used
    
- How they depend on each other
    

This service **automates** that process to save time and reduce errors.

---

### 🛠️ **Two Discovery Methods**

|Method|Description|Use Case|
|---|---|---|
|🔌 **Agentless Discovery**|Uses the **AWS Discovery Connector (VMware only)** to collect info like **VM inventory, config, CPU, memory, disk usage**|Best for quick, lightweight assessments of **VMware environments**|
|🕵️ **Agent-based Discovery**|Install **agents on servers** to collect **detailed info**: running processes, network connections, system config, etc.|Best when you need **deep visibility** into dependencies and performance|

---

### 📊 **Where Does the Data Go?**

- All discovered data can be **viewed and analyzed** inside **AWS Migration Hub**.
    
- Helps create **migration plans** with minimal risk.
    

---

### 🧠 Real-Life Example:

A large enterprise wants to move to AWS but has **hundreds of on-prem servers**.

- They install **Application Discovery Agents** on key systems.
    
- It gathers CPU/memory/network usage and maps **which apps talk to which**.
    
- They use the insights in **AWS Migration Hub** to prioritize and group apps for migration.
    

---

Let me know if you want a diagram of how this fits into a typical migration workflow!

### 🔄 **AWS Application Migration Service (MGN)**

- AWS’s **primary tool for lift-and-shift (rehost)** migrations.
    
- It replaces older tools like **CloudEndure Migration** and **AWS Server Migration Service (SMS)**.
    

---

### ✅ **Key Features**

|Feature|Description|
|---|---|
|🚀 **Lift-and-Shift**|Move servers **as-is** from on-prem, VMs, or other clouds to **EC2 instances on AWS**.|
|🌐 **Platform Support**|Works with **physical servers**, **VMware**, **Hyper-V**, **Azure**, **Google Cloud**, etc.|
|💾 **OS & Database Support**|Supports **Windows**, **Linux**, and many popular databases.|
|🕒 **Minimal Downtime**|Uses **continuous replication**, so servers stay in sync with AWS. Only small cutover downtime needed.|
|💸 **Cost-Effective**|No need to redesign apps; migrate quickly with lower cost and effort.|
|🛠️ **Native Conversion**|Converts source machines to **native AWS EC2 instances** with AWS drivers and settings.|

---

### 🔧 How It Works (Simplified Flow):

1. Install the **MGN agent** on source servers.
    
2. It continuously replicates data to AWS.
    
3. When ready, do a **test launch** on EC2.
    
4. Do a **final cutover** (small downtime), and you're live on AWS!
    

---

### 🧠 Real-Life Example:

A company has 100 on-prem servers running critical apps.

- They install MGN agents, which replicate data to AWS in real time.
    
- After testing, they cut over on a weekend with **minimal downtime**, and the apps start running on EC2.

## AWS Migration Evaluator
• Helps you build a data-driven business case for migration to AWS
• Provides a clear baseline of what your organization is running today
• Install Agentless Collector to conduct broad-based discovery
• Take a snapshot of on-premises foot-print, server dependepncies, …
• Analyze current state, define target state, then develop migration plan

### 🗂️ **AWS Migration Hub**

- A **central dashboard** to help you **plan, track, and manage** your entire **cloud migration** journey.
    
- Think of it as your **migration control center**.
    

---

### ✅ **Key Features**

|Feature|Description|
|---|---|
|📋 **Inventory Collection**|Collects data on your **servers and applications** for assessment and planning.|
|🚀 **Migration Tracking**|Track migration status in **real-time**, including from services like **MGN** (Application Migration) and **DMS** (Database Migration).|
|⚙️ **Lift-and-Shift Automation**|Helps you **automate large migrations**, especially rehosting/lift-and-shift.|
|🔄 **Orchestrator**|**AWS Migration Hub Orchestrator** offers **pre-built migration templates** (e.g., for SAP, SQL Server), saving time and effort.|
|🔗 **Integration**|Works with tools like **Application Discovery Service**, **MGN**, and **DMS** for end-to-end migration management.|

---

### 🧠 Real-Life Example:

Your company is migrating 50 applications to AWS.

- Use **AWS Application Discovery Service** to collect data.
    
- Track the migration of VMs using **MGN** and databases using **DMS**.
    
- Use **Migration Hub** to **monitor all progress in one place**, ensuring smooth coordination across teams.

### 💥 **AWS Fault Injection Simulator (FIS)**

- A **fully managed Chaos Engineering tool** that helps you **test how your AWS applications handle failures**.
    
- Simulates real-world **disruptions** (e.g., high CPU, network delay, service crash) to improve **resilience**.
    

---

### ✅ **Key Features**

|Feature|Description|
|---|---|
|🧪 **Chaos Engineering**|Stress your system on purpose to find weaknesses—just like a fire drill for your app.|
|⚡ **Simulate Failures**|Trigger events like CPU spikes, instance shutdowns, or network latency.|
|🔍 **Find Bottlenecks**|Uncover **hidden bugs**, **scaling issues**, or **slow recovery times**.|
|🧰 **Pre-built Templates**|Use templates to simulate common failure scenarios easily.|
|🔗 **Service Support**|Works with **EC2, ECS, EKS, RDS**, and other AWS services.|
|🔐 **Safe & Controlled**|Experiments can be limited in scope and rolled back—**safe for production** (with caution).|

---

### 🧠 Real-Life Example:

You're running a web app on **EKS (Kubernetes)**.

- Use FIS to simulate a **node failure** or a **pod crash**.
    
- Observe how quickly your app recovers (auto-healing, failover).
    
- Use the insights to **improve auto-scaling rules or monitoring**.

## AWS Step Functions
• Build serverless visual workflow to
orchestrate your Lambda functions
• Features: sequence, parallel, conditions,
timeouts, error handling, …
• Can integrate with EC2, ECS, On
-premises
servers, API Gateway, SQS queues, etc
…
• Possibility of implementing human approval
feature
• Use cases: order fulfillment, data processing,
web applications, any workflow

### **AWS Ground Station**

- A **fully managed service** that lets you **communicate with satellites**, **receive data**, and **process it directly in AWS**.
    
- Think of it as **"AWS for space"**—you can control satellites and work with their data **without building your own ground station**.
    

---

### ✅ **Key Features**

|Feature|Description|
|---|---|
|🌐 **Global Ground Station Network**|AWS has ground stations **close to its regions**, reducing latency and speeding up data delivery.|
|🚀 **Satellite Control**|You can **schedule and manage satellite communication** (e.g., send commands or receive data).|
|⚡ **Fast Data Ingestion**|Download satellite data to your **AWS VPC in seconds**.|
|💾 **Store & Process Data**|Save data to **Amazon S3** or process in **EC2** for real-time insights.|
|📡 **Use Cases**|Satellite-based services like **weather forecasting**, **earth imaging**, **video streaming**, **disaster monitoring**, etc.|
|🔄 **Scalable**|No need to manage ground station hardware; scale with your satellite needs.|

---

### 🧠 Real-Life Example:

A company uses satellites to take **high-resolution images of forests** to monitor wildfires.

- With **AWS Ground Station**, they download image data directly into **S3**, process it with **SageMaker** to detect fire zones, and alert emergency teams—**all within minutes** of the satellite pass.

### 📬 **Amazon Pinpoint**

A **scalable 2-way communication service** designed to help you **engage customers** through **email, SMS, push notifications, voice, and in-app messaging**.

---

### ✅ **Key Features**

|**Feature**|**Description**|
|---|---|
|📢 **Multi-Channel Messaging**|Supports **Email, SMS, Push, Voice, In-app messaging**.|
|🧑‍🤝‍🧑 **Targeted Campaigns**|Create **audience segments**, send **personalized** messages.|
|🔁 **Two-Way Communication**|You can **send** messages and also **receive replies**.|
|📊 **Scalable**|Sends **billions of messages per day**.|
|🧠 **Smart Content Delivery**|Define **message templates**, **delivery times**, and **audience behavior rules**.|
|📈 **Analytics**|Track open rates, delivery stats, and campaign effectiveness.|

---

### 🎯 **Use Cases**

- Run **marketing campaigns**
    
- Send **transactional messages** (e.g., OTPs, order confirmations)
    
- Push **product updates or app alerts**
    
- Conduct **surveys or feedback loops**
    

---

### 🔄 **Pinpoint vs SNS/SES**

|Feature|SNS / SES|Amazon Pinpoint|
|---|---|---|
|Audience Management|You **manually manage** recipients|You create **segments & campaigns**|
|Message Personalization|Basic|Advanced (user attributes, templates)|
|Delivery Scheduling|Manual or custom logic|Built-in scheduling tools|
|Campaign Management|No|Full **campaign creation & analytics**|
|Best for|**Simple messaging tasks**|**Marketing, analytics, personalized engagement**|

---

### 🧠 Real-Life Example:

An **e-commerce app** wants to:

- Send **promotional emails** to customers who haven’t purchased in 30 days.
    
- Send **SMS alerts** for flash sales.
    
- **Track open and click rates** for each campaign.
    

They use **Amazon Pinpoint** to:

- Create **audience segments** based on user behavior.
    
- Design **multi-channel message templates**.
    
- Schedule and run **automated campaigns**.
    
- Analyze performance to improve future campaigns.

---

### ✅ **AWS Well-Architected Framework – 6 Pillars (Definition-wise)**

1. **Operational Excellence**  
    • Ability to run & monitor systems to deliver business value and improve processes.  
    • Emphasizes small, reversible changes and learning from failure.  
    • Uses IaC (Infrastructure as Code), observability, and automation.
    
2. **Security**  
    • Protect systems, information, and assets using risk assessment.  
    • Identity management (IAM), encryption, traceability, and incident response.  
    • Security is applied at every layer of architecture.
    
3. **Reliability**  
    • Ensure workloads recover from failures and scale with demand.  
    • Includes automated recovery, horizontal scaling, and failure testing.
    
4. **Performance Efficiency**  
    • Use resources efficiently as demand and technologies evolve.  
    • Focus on serverless, auto-scaling, and advanced services like ElastiCache.  
    • Encourages experimentation and global deployment.
    
5. **Cost Optimization**  
    • Deliver business value at the lowest cost.  
    • Use managed services, track expenditure, and scale with demand.  
    • Focuses on consumption-based pricing and reducing data center costs.
    
6. **Sustainability**  
    • Minimize environmental impact of workloads.  
    • Use efficient hardware, managed services, and right-size resources.  
    • Supports lifecycle policies and low-energy compute options.
    

---

###### ChatGPT said:

## ✅ **AWS Cloud Adoption Framework (AWS CAF) – Simplified**

🔍 **What It Is:**  
A **guideline and planning tool** that helps organizations move to the cloud smoothly and efficiently using AWS.

🛠️ **Why Use It:**

- Built by AWS experts using real-world lessons
    
- Helps plan your **digital/cloud transformation**
    
- Ensures you’re following AWS **best practices**
    

🎯 **What It Does:**

- Identifies **key capabilities** you need for a successful cloud journey
    
- Organizes these into **6 perspectives**:
    

|Perspective|What It Focuses On|
|---|---|
|**Business**|Business goals, value realization|
|**People**|Skills, change management, training|
|**Governance**|Compliance, risk, and decision-making|
|**Platform**|Technology infrastructure and tools|
|**Security**|Security, identity, and access management|
|**Operations**|Ongoing monitoring and performance|

📌 **Example:**  
If a company wants to migrate to AWS, CAF helps them:

- Align their tech with business goals
    
- Train their staff
    
- Ensure security and compliance
    
- Operate reliably once on cloud
    

Let me know if you'd like a real-life scenario for better understanding!

**CAF Perspectives & Foundational Capabilities – Simplified**

---

### 🏢 **1. Business Perspective**

- **Purpose**: Align **cloud use** with your **business goals**.
    
- **Goal**: Ensure your **cloud investments** support digital transformation and improve **business outcomes** (e.g., faster product delivery, cost savings).
    

📌 _Example_: Use AWS to speed up launching a new product.

---

### 👥 **2. People Perspective**

- **Purpose**: Bridge **technology and people**.
    
- **Focus**: Help organizations **adapt to change**, grow skills, and build a cloud-focused **culture**.
    

🔑 Covers:

- Team structure
    
- Training and upskilling
    
- Leadership & change management
    

📌 _Example_: Reskilling employees to work with AWS instead of legacy tools.

---

### 🛡️ **3. Governance Perspective**

- **Purpose**: Manage **cloud projects** in a safe, controlled way.
    
- **Focus**: Maximize benefits and **reduce risk** (like non-compliance, budget overspend).
    

🔑 Includes:

- Setting policies
    
- Risk management
    
- Budget tracking
    

📌 _Example_: Defining who can launch AWS resources and setting cost limits.

### 🧱 **4. Platform Perspective**

- **Purpose**: Build a **strong cloud foundation** for apps.
    
- **Focus**: Create a **scalable, secure, and modern** cloud setup.
    
- Supports **hybrid** (on-prem + cloud), **modernization**, and **cloud-native apps**.
    

📌 _Example_: Moving an old HR system to AWS or building a new app on serverless tech.

---

### 🔐 **5. Security Perspective**

- **Purpose**: Protect your **data** and **cloud resources**.
    
- **Focus**: Ensure **confidentiality** (only the right people can access), **integrity** (data isn't changed), and **availability** (data is accessible).
    

📌 _Example_: Encrypting customer data in S3 and enforcing IAM policies.

---

### ⚙️ **6. Operations Perspective**

- **Purpose**: Keep your **cloud systems reliable** and **efficient**.
    
- **Focus**: Monitor, manage, and improve **cloud performance**, uptime, and support.
    

📌 _Example_: Setting up CloudWatch alarms to detect when a web app goes down.

---

### ✅ **AWS Right Sizing (Definition-wise)**

• Match EC2 instance size/type to workload needs at lowest cost.  
• Avoid overprovisioning—start small, scale up.  
• Use tools like CloudWatch, Cost Explorer, Trusted Advisor.  
• Important during and after cloud migration.

---

### ✅ **AWS Well-Architected Tool (Definition-wise)**

• Free service to evaluate your architecture using the 6 pillars.  
• Review workload by answering questions.  
• Generates a report and recommends best practices.

---

### ✅ **AWS Customer Carbon Footprint Tool (Definition-wise)**

• Tracks and forecasts AWS-related carbon emissions.  
• Helps meet sustainability goals.  
• Shows emissions by service, geography, and over time.

---

### ✅ **AWS Ecosystem Tools (Definition-wise)**

1. **AWS Blogs** – Official product updates and best practices.
    
2. **AWS Forums** – Community support for AWS queries.
    
3. **AWS Whitepapers** – Technical and strategic cloud guidance.
    
4. **AWS Solutions Library** – Ready-made cloud architecture templates.
    

---

### ✅ **AWS Support Plans (Definition-wise)**

• **Developer** – Basic support via email (business hours).  
• **Business** – 24/7 support for production workloads.  
• **Enterprise** – 15-min response for critical workloads, TAM included.

---

### ✅ **AWS Marketplace (Definition-wise)**

• Online store for 3rd-party AWS-compatible software.  
• Offers AMIs, containers, SaaS, CloudFormation templates.  
• Purchases are included in AWS billing.

---

### ✅ **AWS Training (Definition-wise)**

• Offers Digital, Classroom, Private, and Government training.  
• AWS Academy supports educational institutions.

---

### ✅ **AWS Professional Services & Partner Network (Definition-wise)**

• Professional Services: AWS experts to help with projects.  
• APN (AWS Partner Network):

- **Technology Partners** – Hardware & software
    
- **Consulting Partners** – AWS solution builders
    
- **Training Partners** – AWS learning providers
    

---

### ✅ **AWS IQ (Definition-wise)**

• On-demand AWS help from certified freelancers.  
• Pay per milestone, integrated with AWS billing.  
• Includes chat, video calls, and proposal tracking.

---

### ✅ **AWS re:Post (Definition-wise)**

• Community Q&A platform replacing AWS Forums.  
• Crowd-sourced, expert-reviewed technical answers.  
• Integrated with AWS Premium Support if unanswered.

### EFS – Elastic File System
• Managed NFS (network file system) that can be mounted on 100s of EC2
• EFS works with Linux EC2 instances in multi-AZ
• Highly available, scalable, expensive (3x gp2), pay per use, no capacity planning

## AWS Availability Zones
• Each region has many availability zones
(**usually 3, min is 3, max is 6).** Example:
• ap-southeast-2a
• ap-southeast-2b
• ap-southeast-2c
• **Each availability zone (AZ) is one or more**
**discrete data centers with redundant power**,
networking, and connectivity
• They’re separate from each other, so that
they’re isolated from disasters
• They’re connected with high bandwidth,
ultra-low latency networking

![[Pasted image 20250728152553.png]]


## AWS Professional Services & Partner Network
• The AWS Professional Services organization is a global team of experts
• They work alongside your team and a chosen member of the APN
• APN = AWS Partner Network
• APN Technology Partners: providing hardware, connectivity, and software
• APN Consulting Partners: professional services firm to help build on AWS
• APN Training Partners: find who can help you learn AWS
• AWS Competency Program: AWS Competencies (Kaabil) are granted to APN
Partners who have demonstrated technical proficiency and proven
customer success in specialized solution areas.
• AWS Navigate Program: help Partners become better Partners

### **AWS Artifact (Simplified)**

_(Note: Not a service, it's a portal)_

---

🔹 **What it is:**  
A **portal** to access **AWS compliance documents** and **legal agreements**

---

🔹 **Artifact Reports**  
– **Download** AWS **security & compliance reports**  
– From **third-party auditors** (like ISO, SOC, PCI reports)  
📌 _Use case_: Internal audits, compliance checks

---

🔹 **Artifact Agreements**  
– **Review, accept, and track** AWS legal agreements  
– Examples:

- **BAA** (Business Associate Addendum)
    
- **HIPAA** (Health rules)  
    📌 _For individual accounts or whole organization_
    

---

🔹 **Why use it?**  
– Support **compliance efforts**  
– Show proof of AWS security standards to stakeholders

---
## **EC2 Instance Purchasing Options (Simplified)**

### 1. **On-Demand Instances**

- **Definition**: Pay for compute capacity by the second/minute with no long-term commitments.
    
- **Use Case**: Ideal for short-term workloads or unpredictable traffic.
    
- **Real-Life Example**: Like booking a taxi instantly—pay only for the ride, no need for a subscription or plan.
    

---

### 2. **Reserved Instances (RI)**

- **Definition**: Commit to using EC2 for 1 or 3 years in exchange for a significant discount (up to 72%) over on-demand pricing.
    
- **Types**:
    
    - **Standard RI** – More discount, less flexibility.
        
    - **Convertible RI** – Slightly less discount but allows changing instance types.
        
- **Use Case**: Good for steady-state or predictable workloads (e.g., web servers).
    
- **Real-Life Example**: Like buying a yearly gym membership—cheaper than paying daily if you go regularly.
    

---

### 3. **Spot Instances**

- **Definition**: Buy unused EC2 capacity at up to 90% discount compared to On-Demand prices.
    
- **Limitation**: Can be terminated by AWS with a 2-minute warning if capacity is needed elsewhere.
    
- **Use Case**: Best for fault-tolerant and flexible applications (batch processing, big data, testing).
    
- **Real-Life Example**: Like last-minute flight tickets—cheap but can be canceled anytime.
    

---

### 4. **Savings Plans**

- **Definition**: Flexible pricing model offering lower prices in exchange for a 1- or 3-year commitment to a consistent amount of usage (measured in $/hour).
    
- **Types**:
    
    - **Compute Savings Plan** – Most flexible across instance families, regions, and OS.
        
    - **EC2 Instance Savings Plan** – Less flexible but more discounted, tied to instance family in a region.
        
- **Use Case**: Great if you want discounts like RI but with more flexibility.
    
- **Real-Life Example**: Like a power-saving plan—commit to monthly usage and pay less per unit.
  
  ## **1. Dedicated Hosts**

- **Definition**: A physical EC2 server fully dedicated to your use. You control instance placement and visibility into sockets, cores, and host-level configurations.
    
- **Billing**: Pay per host, not per instance.
    
- **Use Case**:
    
    - For meeting compliance requirements (e.g., software licenses tied to physical hardware like Microsoft/Oracle).
        
    - Visibility and control over instance placement.
        
- **Real-Life Example**:  
    Like renting a **whole hotel floor**—you decide who stays in which room and have full control of that floor.
    

---

## 🔹 **2. Capacity Reservations**

- **Definition**: Reserve EC2 capacity in a specific Availability Zone for a future time, without changing how you pay (works with On-Demand or RIs).
    
- **Billing**: You are charged whether you use the capacity or not, once the reservation is active.
    
- **Use Case**:
    
    - Ensures instance availability for critical workloads (especially in high-demand times).
        
    - Ideal for disaster recovery, scheduled scaling, or ensuring resources during events like Black Friday.
        
- **Real-Life Example**:  
    Like **reserving seats on a flight**—you’ve paid in advance to ensure you have a seat, even if others are overbooked.
