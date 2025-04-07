## Project Overview

### Scenario

You’ve been hired as a Salesforce Administrator for the **State of Missouri**. Leadership in multiple state departments wants a **central CRM** to track a variety of **government projects, initiatives, and resource allocations**. You will build out a solution in a Trailhead Playground that helps different agencies manage their operational tasks, get alerts when deadlines or budget thresholds approach, and monitor dashboards showing up-to-date metrics. 

### Learning Objectives

1. **Familiarize yourself with Custom Objects** and Custom Fields relevant to state government operations.
2. **Practice process automation** (Flows or Workflow Rules) for key triggers, e.g., approaching deadlines or budget constraints.
3. **Create a Dashboard** to display metrics such as total projects by department, tasks nearing deadlines, and allocated budget usage.
4. **(Optional Advanced)** Expand with additional flows, an approval process for budget or resource requests, or integration-like scenarios.

---

## Step-by-Step Project Requirements

Below are core (beginner-level) requirements, followed by **Optional Advanced** expansions if you want more complexity.

### 1. Set Up the Data Model

1. **Create a Custom Object** named **Project** (or **State Project**).
   - Plural Label: **Projects**
   - Record Name: **Project Number** (Auto Number) or simply a text Name field
2. **Add Custom Fields** on the **Project** object:
   - **Name** (Text) – The name of the project or initiative (e.g., “Highway Renovation 2024”).
   - **Department** (Picklist) – Possible values: Transportation, Education, Health & Senior Services, Public Safety, etc.
   - **Current Budget Allocation** (Currency) – The approved or assigned budget for this project.
   - **Budget Threshold** (Currency) – A lower boundary or threshold that triggers an alert if the project’s remaining funds approach this limit.
   - **Deadline** (Date) – The date by which this project should be completed.
   - **Key Stakeholder** (Lookup to Contact, for instance) – So you can associate the project with the relevant official or manager (using standard Contact records).
3. (Optional but recommended) **Create a Custom Object Tab** for **Project** so it’s easy to navigate in the Salesforce app.
4. **Relate Projects to “Agencies” or “Departments”** (optional design choice):
   - If you want to track various agencies or sub-departments as standard “Accounts,” you could add a **Lookup** to Account from the Project object.
   - Alternatively, create a **Department** custom object, then add a **Master-Detail** or **Lookup** relationship from **Project** to **Department** so each project is tied to a specific area of government.

**Data Model - Beginner Summary**:

- **Object**: Project (custom).
- **Fields**: Name, Department (picklist), Current Budget Allocation, Budget Threshold, Deadline, Key Stakeholder (Contact lookup).
- **Optional**: Agency or Department object for more robust relationships.

**Optional Advanced**:
- Create fields for **Project Stage** (Not Started, In Progress, Completed), **Est. Completion %**, or other performance indicators.
- Implement **Validation Rules** ensuring the Budget Threshold isn’t greater than the Current Budget Allocation, or that Deadline can’t be in the past.

---

### 2. Build Automation for Key Alerts

Missouri government leadership wants to ensure they receive **timely alerts** if a project is nearing its **budget threshold** or is **close to its deadline** without completion.

#### A) Choice #1: Workflow Rule (Beginner)

1. **Create a Workflow Rule** on the Project object:
   - **Criteria**: 
     - When the `Current Budget Allocation` goes below `Budget Threshold`, OR  
     - The `Deadline` is within a certain number of days (you might store a formula or a date check).
   - **Email Alert**: Notify the Project Manager or relevant stakeholder.
2. **Activate** the workflow rule.

#### B) Choice #2: Flow (Beginner-Intermediate)

1. **Create a Record-Triggered Flow** on the Project object.  
   - **Trigger**: When a record is created or updated.
   - **Condition**: 
     - `Current Budget Allocation < Budget Threshold` (check for budget issues), OR  
     - `Deadline - TODAY() <= X` (check if the project is nearing its due date).
   - **Action**: Send an Email Alert or Post to Chatter to notify the relevant contact or official. 
2. (Optional) If you want deeper automation, the Flow could **create a Task** for the relevant manager to review or escalate the project.

**Optional Advanced**:
- **Multi-step Flow**: 
  - If the project’s Budget is too low or the Deadline is imminent, create a **Case** or **Opportunity**-like record (for budgeting) that requires manager approval.  
- **Approval Process**: 
  - If more funds are needed, automatically launch an Approval Process that routes to the Department Director. 
- **Scheduled Paths in Flow**: 
  - For instance, if the deadline is 7 days away and the project is under 50% complete, send a reminder.

---

### 3. Create Reports & Dashboards

Missouri state officials want an **at-a-glance view** of projects across all departments.

1. **Create Custom Report Types**
   - Base the Report Type on the **Project** object.
   - (Optional) If you created a Department object, include it in the relationships for your report type.
2. **Build Reports** in a **Projects** folder:
   - **Low Budget Projects**: Filter where `Current Budget Allocation < Budget Threshold`.
   - **Projects by Department**: Group by the **Department** field to see how many (and which) projects each department oversees.
   - **Upcoming Deadlines**: Filter to show projects whose `Deadline` is within the next 30 days.
3. **Create a Dashboard**
   - Add components referencing the above reports.
   - Consider:
     - **Gauge** for total allocated vs. used budget (if you store used funds).
     - **Donut Chart** by Department.
     - **Table** listing projects with deadlines in the next 14 days.
4. **Set Dashboard Filters** (optional):
   - E.g., Filter by Department or by a date range for project deadlines.

**Optional Advanced**:
- **Dynamic Dashboards**: Let each Department Director see only their own projects.
- **Lightning App Page**: Embed a subset of these charts on the Project record page or a custom homepage for quick reference.
- **Schedule Dashboard Refresh**: Email out the updated snapshot weekly to Department Heads.

---

### 4. Testing and Deployment

In your **Trailhead Playground**:

1. **Add Sample Data**:
   - Create ~15 Project records across different departments (e.g., Transportation, Education, Health, etc.).
   - Enter realistic budget allocations and deadlines (like “Bridge Repair 2024” with a specific date and budget).
2. **Trigger the Automation**:
   - Manually update a Project so that the `Current Budget Allocation` is below the `Budget Threshold`.
   - Verify that your email or Flow notification goes to the designated user.
   - If you’re testing deadlines, set a `Deadline` to today or tomorrow and confirm that an alert is triggered.
3. **Check Your Reports and Dashboards**:
   - Ensure that “Low Budget Projects” or “Upcoming Deadlines” are visible.
4. **Documentation**:
   - Write short notes describing how your **Project** object is set up, how the Flow or Workflow handles alerts, and how your dashboards highlight potential project risks.

---

## Putting It All Together

When completed, you should have:

1. A **custom Project** object, possibly a **Department** or **Agency** object for organizational grouping.
2. **Custom Fields** tracking critical data (budget, threshold, deadlines, stakeholder).
3. **Automation** that notifies relevant contacts when budgets or deadlines are at risk.
4. **Reports & Dashboards** enabling real-time visibility into project progress and departmental status.

---

## Optional Advanced Add-Ons

- **Integration**: In a real scenario, the state might integrate Salesforce with finance or procurement systems to sync actual spend vs. allocated budget. Simulate such an integration or mention how you’d connect.
- **Role-Based Security**: Only certain profiles (e.g., Department Heads) can edit budget fields; others can view only. This can be enforced with field-level security and sharing rules.
- **Public-Facing Site**: If the state wants partial transparency, you could experiment with Experience Cloud to expose select project statuses to the public.

---

## Next Steps & Extensions

1. **Extend the Data Model** to handle more complex aspects: sub-projects, vendor relationships, or multi-year budgeting.
2. **Dive Deeper into Lightning Web Components (LWCs)** if you’d like more user-friendly forms or custom UI for project management.
3. **Explore Larger Data Volumes**: For an advanced challenge, import dozens or hundreds of project records (real or demo data) to see how your dashboards and processes scale.

---

### Project Recap

- **Difficulty**: Beginner-friendly with direct data modeling, but can be enhanced with advanced flows and integrated processes.
- **Scenario**: Missouri state government needs a CRM to manage multi-department projects, keep budgets in check, and monitor deadlines.
- **Key Deliverables**:
  1. Custom **Project** object + fields
  2. Basic or advanced **Automation** (Workflow/Flow) for budget/deadline alerts
  3. **Reports & Dashboards** summarizing departmental progress and resource usage

This CRM solution for the **State of Missouri Government** will strengthen your Salesforce Admin skills in **custom data modeling, process automation,** and **reporting**—while demonstrating practical, real-world usage in a midwest U.S. government setting. Enjoy building out your solution in your Trailhead Playground!

