# Generate Weekly Timesheet Report

Generate a summarized report of hours logged from work items during a specified week, with flexible filtering options for closed, worked on, or both types of tasks. Report is organized in a hierarchical tree structure by Feature > User Story > Task.

## Instructions

This command queries Azure DevOps for work items based on flexible filtering criteria (closed, worked on, or both) during a specified time period and generates a timesheet report showing the "Completed Work" hours rolled up in a hierarchical tree.

### Phase 1: Validate Azure DevOps Configuration

Before proceeding with report generation, verify that Azure DevOps configuration exists:

1. Use the **Read tool** to read `CLAUDE.md` from the project root
2. If the file doesn't exist OR doesn't contain an "## Azure DevOps" section:
   - Display the following error message:
     ```
     ❌ Azure DevOps configuration not found

     CLAUDE.md does not contain Azure DevOps configuration.

     Please run /ado-init first to configure Azure DevOps settings for this project.

     The /ado-init command will:
     - Configure your organization, project, and team
     - Set up Area Path and Iteration Path defaults
     - Define naming conventions and work item guidelines
     - Optionally configure the Azure DevOps MCP server
     ```
   - **STOP** and do not proceed further

3. If Azure DevOps section exists:
   - Parse the CLAUDE.md content to extract configuration values:
     - Organization name (look for text after 'Organization:**')
     - Project name (look for text after 'Project:**')
   - Store these values for use in work item queries
   - Proceed to Phase 2

**IMPORTANT**:
- Use **Read tool** to check CLAUDE.md - DO NOT use bash commands
- The entire command should STOP if Azure DevOps configuration is not found
- Do not prompt the user for configuration values - they must run /ado-init first

### Phase 2: Gather Report Parameters

Collect report parameters from the user using the AskUserQuestion tool to ask multiple questions at once.

**Step 1 - Report Configuration (4 questions combined):**

Use the **AskUserQuestion tool** to ask all report configuration questions at once:

```json
{
  "questions": [
    {
      "question": "What is your organization's work week definition?",
      "header": "Week Type",
      "multiSelect": false,
      "options": [
        {
          "label": "Monday-Sunday",
          "description": "ISO standard week (Monday through Sunday)"
        },
        {
          "label": "Sunday-Saturday",
          "description": "Sunday through Saturday week"
        }
      ]
    },
    {
      "question": "Which time period would you like to report on?",
      "header": "Time Period",
      "multiSelect": false,
      "options": [
        {
          "label": "Current week",
          "description": "Report on the current week"
        },
        {
          "label": "Last week",
          "description": "Report on last week"
        },
        {
          "label": "Specific week",
          "description": "Provide a specific end date for the week"
        }
      ]
    },
    {
      "question": "What types of tasks would you like to include in the report?",
      "header": "Task Filter",
      "multiSelect": false,
      "options": [
        {
          "label": "Closed only",
          "description": "Only tasks that were closed during the time period"
        },
        {
          "label": "Worked on only",
          "description": "Only tasks that were worked on (but not closed) during the time period"
        },
        {
          "label": "Both",
          "description": "Both closed and worked-on tasks during the time period"
        }
      ]
    },
    {
      "question": "Which date field should be used to filter tasks within the time period? (If you selected 'Worked on only', 'Changed Date' is recommended)",
      "header": "Date Field",
      "multiSelect": false,
      "options": [
        {
          "label": "Closed Date",
          "description": "When the task was marked as closed (best for 'closed only' filter)"
        },
        {
          "label": "Changed Date",
          "description": "When the task was last updated (best for 'worked on' or 'both' filters)"
        }
      ]
    }
  ]
}
```

Wait for the user's response before proceeding.

**Step 1a - End Date (only if user chose "Specific week"):**

If the user chose "Specific week" in Step 1, simply output the following text as your response message and STOP (DO NOT call any tools):

"What is the end date for the week you want to report on?

Please provide the date in YYYY-MM-DD format (e.g., 2025-01-17 for the week ending January 17, 2025)."

Wait for the user's next message with the end date before proceeding.

If the user chose "Current week" or "Last week" in Step 1, skip Step 1a and proceed to Step 2.

**Step 2 - Display Options (3 questions combined):**

After handling the optional end date, use the **AskUserQuestion tool** to ask display-related questions:

```json
{
  "questions": [
    {
      "question": "What level of detail would you like in the report?",
      "header": "Verbosity",
      "multiSelect": false,
      "options": [
        {
          "label": "Level 1",
          "description": "Work Item ID & Hours Only"
        },
        {
          "label": "Level 2",
          "description": "Work Item ID, Title, and Hours"
        },
        {
          "label": "Level 3",
          "description": "Work Item ID, Title, Description, and Hours"
        }
      ]
    },
    {
      "question": "How would you like to organize the report?",
      "header": "Grouping",
      "multiSelect": false,
      "options": [
        {
          "label": "By hierarchy",
          "description": "Group by Feature > User Story > Task (traditional tree view)"
        },
        {
          "label": "By date",
          "description": "Group by day of the week with work items under each date"
        },
        {
          "label": "By date with hierarchy",
          "description": "Group by day, then show Feature > User Story > Task within each day"
        }
      ]
    },
    {
      "question": "Whose hours would you like to report on?",
      "header": "User",
      "multiSelect": false,
      "options": [
        {
          "label": "Current user",
          "description": "Current authenticated user (default)"
        },
        {
          "label": "Specific team member",
          "description": "Provide a specific team member's name"
        }
      ]
    }
  ]
}
```

Wait for the user's response before proceeding.

**Step 2a - Team Member Name (only if user chose "Specific team member"):**

If the user chose "Specific team member" in Step 2, simply output the following text as your response message and STOP (DO NOT call any tools):

"What is the name of the team member to report on?

Please provide the full name as it appears in Azure DevOps (e.g., 'John Smith')."

Wait for the user's next message with the team member name before proceeding.

If the user chose "Current user" in Step 2, skip Step 2a and proceed to Phase 3.

**IMPORTANT**:
- Use **AskUserQuestion tool** twice:
  - Step 1: Ask 4 report configuration questions together (week definition, time period, task filter type, date field)
  - Step 2: Ask 3 display option questions together (verbosity level, grouping mode, user selection)
- Use simple text output for conditional free-form inputs:
  - Step 1a: End date (only if "Specific week" was selected)
  - Step 2a: Team member name (only if "Specific team member" was selected)
- Wait for the user's response after each step before proceeding
- Store all collected values for use in Phase 3, 4, 5, and 6:
  - Week definition (Monday-Sunday or Sunday-Saturday)
  - Time period (Current week, Last week, or Specific week)
  - End date (if Specific week selected)
  - Task filter type (Closed only, Worked on only, or Both)
  - Date field (Closed Date or Changed Date)
  - Verbosity level (Level 1, Level 2, or Level 3)
  - Grouping mode (By hierarchy, By date, or By date with hierarchy)
  - User selection (Current user or team member name)
- Calculate the date range based on user's choices:
  - For "Current week": Calculate current week's start and end dates
  - For "Last week": Calculate last week's start and end dates
  - For "Specific week": Calculate the week's start date from the provided end date
- Week calculation should use the week definition from Step 1

### Phase 3: Calculate Date Range

Based on the user's time period selection and week definition, calculate the start and end dates:

1. **Determine today's date** using the current system date
   - The system provides the current date in `<env>` section as "Today's date: YYYY-MM-DD"
   - This is in ISO 8601 format where YYYY-MM-DD means Year-Month-Day
   - For example: 2025-10-23 is October 23, 2025 (NOT January 23)
   - **IMPORTANT**: Trust this date exactly as provided - do NOT second-guess or reinterpret the format
   - The month is always the middle number (01=January, 02=February, ... 10=October, 11=November, 12=December)

2. **Calculate date range** based on user selections:

   **For "Current week":**

   Use the following algorithm to calculate the current week:

   - If week is Monday-Sunday:
     1. Determine today's day of the week (Monday=1, Tuesday=2, Wednesday=3, Thursday=4, Friday=5, Saturday=6, Sunday=7)
     2. Calculate days to subtract from today to get to Monday: (day_of_week - 1)
        - If today is Monday (day_of_week=1): subtract 0 days (today is the start)
        - If today is Tuesday (day_of_week=2): subtract 1 day
        - If today is Wednesday (day_of_week=3): subtract 2 days
        - If today is Thursday (day_of_week=4): subtract 3 days
        - If today is Friday (day_of_week=5): subtract 4 days
        - If today is Saturday (day_of_week=6): subtract 5 days
        - If today is Sunday (day_of_week=7): subtract 6 days
     3. Start date = Today - days_to_subtract (this gives you the Monday of the current week)
     4. End date = Start date + 6 days (this gives you the Sunday of the current week)

   - If week is Sunday-Saturday:
     1. Determine today's day of the week (Sunday=0, Monday=1, Tuesday=2, Wednesday=3, Thursday=4, Friday=5, Saturday=6)
     2. Calculate days to subtract from today to get to Sunday: day_of_week
        - If today is Sunday (day_of_week=0): subtract 0 days (today is the start)
        - If today is Monday (day_of_week=1): subtract 1 day
        - If today is Tuesday (day_of_week=2): subtract 2 days
        - If today is Wednesday (day_of_week=3): subtract 3 days
        - If today is Thursday (day_of_week=4): subtract 4 days
        - If today is Friday (day_of_week=5): subtract 5 days
        - If today is Saturday (day_of_week=6): subtract 6 days
     3. Start date = Today - days_to_subtract (this gives you the Sunday of the current week)
     4. End date = Start date + 6 days (this gives you the Saturday of the current week)

   **Example for Monday-Sunday week:**
   - If today is October 23, 2025 (Thursday):
     - Day of week = 4 (Thursday)
     - Days to subtract = 4 - 1 = 3 days
     - Start date = Oct 23 - 3 days = October 20, 2025 (Monday)
     - End date = Oct 20 + 6 days = October 26, 2025 (Sunday)
     - **Result: October 20-26, 2025**

   **For "Last week":**

   - If week is Monday-Sunday:
     1. Calculate the current week's Monday using the algorithm above
     2. Start date = Current week's Monday - 7 days (Monday of last week)
     3. End date = Start date + 6 days (Sunday of last week)

   - If week is Sunday-Saturday:
     1. Calculate the current week's Sunday using the algorithm above
     2. Start date = Current week's Sunday - 7 days (Sunday of last week)
     3. End date = Start date + 6 days (Saturday of last week)

   **For "Specific week":**
   - End date: User-provided date
   - If week is Monday-Sunday:
     - Validate that the end date is a Sunday
     - Start date: 6 days before the end date (the Monday)
   - If week is Sunday-Saturday:
     - Validate that the end date is a Saturday
     - Start date: 6 days before the end date (the Sunday)

3. **Format dates** in ISO 8601 format (YYYY-MM-DD) for use in WIQL queries

**IMPORTANT**:
- All date calculations should be done programmatically (don't ask user for date calculations)
- **CRITICAL**: Follow the algorithm exactly as specified - do NOT add or subtract extra days
  - For Monday-Sunday: Start = Today - (day_of_week - 1), End = Start + 6
  - Verify your calculation matches the example: Oct 23 (Thu) → Oct 20-26 (Mon-Sun)
- For specific week option, validate that the end date matches the expected day of week
- If validation fails, display helpful error message and ask user to provide correct end date
- Store calculated start_date and end_date for use in Phase 4

### Phase 4: Query Azure DevOps for Work Items

Query Azure DevOps for work items based on the user's filter choices:

1. **Build WIQL query** dynamically based on Phase 2 selections:

   **Base SELECT clause** (always include):
   ```
   SELECT [System.Id], [System.WorkItemType], [System.Title], [System.Description],
          [System.Parent], [Microsoft.VSTS.Scheduling.CompletedWork],
          [System.AssignedTo], [Microsoft.VSTS.Common.ClosedDate], [System.ChangedDate], [System.State]
   FROM WorkItems
   ```

   **WHERE clause** - Build based on user's choices:

   **Date Field Selection** (from Step 4):
   - If user chose "Closed Date": Use `[Microsoft.VSTS.Common.ClosedDate]` as the date field
   - If user chose "Changed Date": Use `[System.ChangedDate]` as the date field
     - Note: If `System.ChangedDate` fails, try `[Microsoft.VSTS.Common.ChangedDate]` as an alternative

   **Task Filter Type** (from Step 3):

   **"Closed only" - Only closed tasks:**
   ```
   WHERE [System.TeamProject] = '[PROJECT]'
     AND [System.State] = 'Closed'
     AND [[DATE_FIELD]] >= '[START_DATE]T00:00:00.000Z'
     AND [[DATE_FIELD]] <= '[END_DATE]T23:59:59.999Z'
     AND [Microsoft.VSTS.Scheduling.CompletedWork] > 0
   ```

   **"Worked on only" - Only worked on (not closed) tasks:**
   ```
   WHERE [System.TeamProject] = '[PROJECT]'
     AND [System.State] <> 'Closed'
     AND [System.ChangedDate] >= '[START_DATE]T00:00:00.000Z'
     AND [System.ChangedDate] <= '[END_DATE]T23:59:59.999Z'
     AND [Microsoft.VSTS.Scheduling.CompletedWork] > 0
   ```
   Note: For this filter, always use ChangedDate regardless of user's date field choice, as non-closed tasks won't have a ClosedDate in the time period.

   **"Both" - Both closed and worked on tasks:**
   ```
   WHERE [System.TeamProject] = '[PROJECT]'
     AND [[DATE_FIELD]] >= '[START_DATE]T00:00:00.000Z'
     AND [[DATE_FIELD]] <= '[END_DATE]T23:59:59.999Z'
     AND [Microsoft.VSTS.Scheduling.CompletedWork] > 0
   ```

   **ORDER BY clause** (always include):
   ```
   ORDER BY [System.Parent] ASC, [System.WorkItemType] ASC, [System.Id] ASC
   ```

   Replace placeholders:
   - `[PROJECT]` → Project name from Phase 1 configuration
   - `[DATE_FIELD]` → `Microsoft.VSTS.Common.ClosedDate` or `System.ChangedDate` based on user choice
   - `[START_DATE]` → Start date from Phase 3 (YYYY-MM-DD format)
   - `[END_DATE]` → End date from Phase 3 (YYYY-MM-DD format)

2. **Execute WIQL query** using the **wit_query** MCP tool:
   - `wiql`: The WIQL query built in step 1
   - `project`: Project name from Phase 1 configuration

3. **Filter work items by user** (if specified):
   - If user chose "Current user": Keep all work items assigned to the current authenticated user
   - If user chose "Specific team member": Keep only work items assigned to the specified team member
   - Check the `System.AssignedTo` field against the user selection

4. **Extract work item data** from query results:
   - For each work item, extract:
     - ID (`System.Id`)
     - Type (`System.WorkItemType`)
     - Title (`System.Title`)
     - Description (`System.Description`) - only if verbosity level 3
     - Parent ID (`System.Parent`)
     - Completed Work hours (`Microsoft.VSTS.Scheduling.CompletedWork`)
     - Assigned To (`System.AssignedTo`)
     - Work Date - Extract the date field that was used in the query:
       - If "Closed Date" filter was used: Extract `Microsoft.VSTS.Common.ClosedDate`
       - If "Changed Date" filter was used: Extract `System.ChangedDate`
       - Parse the date to get day of week (for date-based grouping)

5. **Store work items** in a data structure for tree building

**IMPORTANT**:
- Use **wit_query** MCP tool to execute the WIQL query
- Build the WIQL query dynamically based on user's task filter type and date field choices
- For "worked on only" filter, always use ChangedDate (override user's date field choice if needed)
- **Critical**: Use the correct field name `Microsoft.VSTS.Common.ClosedDate` (NOT `System.ClosedDate`) for Closed Date
- Dates must be in ISO 8601 format with time zone (use UTC: T00:00:00.000Z)
- Always filter by CompletedWork > 0 to only include work items with logged hours
- Filter by State based on task filter type:
  - "Closed only": State = 'Closed'
  - "Worked on only": State <> 'Closed'
  - "Both": No state filter (include all states)
- Filter by user assignment after retrieving results
- Extract Completed Work field - this contains the logged hours
- Handle cases where work items have no parent (Parent field is null/empty)
- Extract State field to display in report if needed

### Phase 5: Organize Work Items Based on Grouping Mode

Organize the work items based on the user's selected grouping mode:

**If user chose "By hierarchy":**

1. **Create parent-child relationships**:
   - Group work items by their Parent ID
   - Build a tree structure where:
     - Features are top-level nodes (no parent or parent is Epic)
     - User Stories are children of Features
     - Tasks, Bugs, Issues are children of User Stories
     - Work items with no parent go under "No Parent" group

2. **Calculate rolled-up hours**:
   - For each parent node (Feature, User Story):
     - Sum the Completed Work hours of all child work items
     - Recursively sum hours from all descendants
   - Only leaf nodes (Tasks, Bugs, Issues) have direct hours
   - Parent hours are the sum of their children's hours

3. **Sort the tree**:
   - Sort Features by ID (ascending)
   - Within each Feature, sort User Stories by ID (ascending)
   - Within each User Story, sort Tasks/Bugs/Issues by ID (ascending)
   - "No Parent" group appears at the end

**If user chose "By date":**

1. **Group work items by date**:
   - Extract the date portion from the Work Date field (ignore time)
   - Group all work items by their date
   - Create a flat list structure (no hierarchy)

2. **Calculate daily totals**:
   - Sum the Completed Work hours for all work items on each date
   - Only include work items with CompletedWork > 0

3. **Sort by date**:
   - Sort dates chronologically (earliest to latest)
   - Within each date, sort work items by ID (ascending)

**If user chose "By date with hierarchy":**

1. **Group work items by date first**:
   - Extract the date portion from the Work Date field (ignore time)
   - Group all work items by their date

2. **Build hierarchy within each date**:
   - For work items on each date, create parent-child relationships
   - Build a tree structure where:
     - Features are top-level nodes (no parent or parent is Epic)
     - User Stories are children of Features
     - Tasks, Bugs, Issues are children of User Stories
     - Work items with no parent go under "No Parent" group

3. **Calculate hours**:
   - Calculate rolled-up hours within each date's hierarchy
   - Calculate total hours for each date (sum of all work items that day)

4. **Sort**:
   - Sort dates chronologically (earliest to latest)
   - Within each date, sort Features by ID, then User Stories, then Tasks

**IMPORTANT**:
- Handle orphaned work items (no parent) appropriately for each grouping mode
- Only count hours from work items with Completed Work > 0
- For hierarchy modes: Parent nodes show sum of child hours
- For date modes: Extract date correctly from the date field used in the query
- Maintain proper hierarchy: Feature > User Story > Task/Bug/Issue
- Work items can be of any type (Task, Bug, Issue, etc.) as long as they have hours

### Phase 6: Display Timesheet Report

Generate and display the timesheet report based on the grouping mode and verbosity level:

**Report Header:**
```
📊 Timesheet Report
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📅 Period: [START_DATE] to [END_DATE] ([WEEK_TYPE])
👤 User: [USER_NAME]
🔍 Filter: [FILTER_DESCRIPTION]
📅 Date Field: [DATE_FIELD_NAME]
⏱️  Total Hours: [TOTAL_HOURS]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Replace placeholders:
- `[START_DATE]` → Formatted start date (e.g., "Jan 13, 2025")
- `[END_DATE]` → Formatted end date (e.g., "Jan 19, 2025")
- `[WEEK_TYPE]` → "Monday-Sunday" or "Sunday-Saturday"
- `[USER_NAME]` → Name of user being reported on
- `[FILTER_DESCRIPTION]` → "Closed only" / "Worked on only" / "Both closed and worked on" (based on Step 3 choice)
- `[DATE_FIELD_NAME]` → "Closed Date" / "Changed Date" (based on Step 4 choice, or "Changed Date" if worked on only)
- `[TOTAL_HOURS]` → Sum of all hours in the report

**Tree Structure Format:**

**Verbosity Level 1 (ID & Hours Only):**
```
📦 Feature [FEATURE_ID] (Total: [FEATURE_HOURS]h)
  📋 Story [STORY_ID] (Total: [STORY_HOURS]h)
    ✓ Task [TASK_ID]: [TASK_HOURS]h
    ✓ Bug [BUG_ID]: [BUG_HOURS]h
  📋 Story [STORY_ID] (Total: [STORY_HOURS]h)
    ✓ Task [TASK_ID]: [TASK_HOURS]h

📦 Feature [FEATURE_ID] (Total: [FEATURE_HOURS]h)
  📋 Story [STORY_ID] (Total: [STORY_HOURS]h)
    ✓ Task [TASK_ID]: [TASK_HOURS]h

🔍 No Parent
  📋 Story [STORY_ID] (Total: [STORY_HOURS]h)
    ✓ Task [TASK_ID]: [TASK_HOURS]h
  ✓ Task [TASK_ID]: [TASK_HOURS]h
```

**Verbosity Level 2 (ID, Title, & Hours):**
```
📦 Feature [FEATURE_ID]: [FEATURE_TITLE] (Total: [FEATURE_HOURS]h)
  📋 Story [STORY_ID]: [STORY_TITLE] (Total: [STORY_HOURS]h)
    ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h
    ✓ Bug [BUG_ID]: [BUG_TITLE] - [BUG_HOURS]h
  📋 Story [STORY_ID]: [STORY_TITLE] (Total: [STORY_HOURS]h)
    ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h

📦 Feature [FEATURE_ID]: [FEATURE_TITLE] (Total: [FEATURE_HOURS]h)
  📋 Story [STORY_ID]: [STORY_TITLE] (Total: [STORY_HOURS]h)
    ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h

🔍 No Parent
  📋 Story [STORY_ID]: [STORY_TITLE] (Total: [STORY_HOURS]h)
    ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h
  ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h
```

**Verbosity Level 3 (ID, Title, Description, & Hours):**
```
📦 Feature [FEATURE_ID]: [FEATURE_TITLE] (Total: [FEATURE_HOURS]h)
  📋 Story [STORY_ID]: [STORY_TITLE] (Total: [STORY_HOURS]h)
    ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h
      📝 [TASK_DESCRIPTION]
    ✓ Bug [BUG_ID]: [BUG_TITLE] - [BUG_HOURS]h
      📝 [BUG_DESCRIPTION]
  📋 Story [STORY_ID]: [STORY_TITLE] (Total: [STORY_HOURS]h)
    ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h
      📝 [TASK_DESCRIPTION]

📦 Feature [FEATURE_ID]: [FEATURE_TITLE] (Total: [FEATURE_HOURS]h)
  📋 Story [STORY_ID]: [STORY_TITLE] (Total: [STORY_HOURS]h)
    ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h
      📝 [TASK_DESCRIPTION]

🔍 No Parent
  📋 Story [STORY_ID]: [STORY_TITLE] (Total: [STORY_HOURS]h)
    ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h
      📝 [TASK_DESCRIPTION]
  ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h
    📝 [TASK_DESCRIPTION]
```

---

**"By date" Grouping Mode Formats:**

When user selects "By date" grouping, work items are organized by date with a flat list (no hierarchy) under each date.

**Verbosity Level 1 (ID & Hours Only):**
```
📅 Monday, Oct 21, 2025 (Total: [DATE_HOURS]h)
  • [WORK_ITEM_ID]: [HOURS]h
  • [WORK_ITEM_ID]: [HOURS]h
  • [WORK_ITEM_ID]: [HOURS]h

📅 Tuesday, Oct 22, 2025 (Total: [DATE_HOURS]h)
  • [WORK_ITEM_ID]: [HOURS]h
  • [WORK_ITEM_ID]: [HOURS]h

📅 Wednesday, Oct 23, 2025 (Total: [DATE_HOURS]h)
  • [WORK_ITEM_ID]: [HOURS]h
```

**Verbosity Level 2 (ID, Title, & Hours):**
```
📅 Monday, Oct 21, 2025 (Total: [DATE_HOURS]h)
  • [WORK_ITEM_ID]: [WORK_ITEM_TITLE] - [HOURS]h
  • [WORK_ITEM_ID]: [WORK_ITEM_TITLE] - [HOURS]h
  • [WORK_ITEM_ID]: [WORK_ITEM_TITLE] - [HOURS]h

📅 Tuesday, Oct 22, 2025 (Total: [DATE_HOURS]h)
  • [WORK_ITEM_ID]: [WORK_ITEM_TITLE] - [HOURS]h
  • [WORK_ITEM_ID]: [WORK_ITEM_TITLE] - [HOURS]h

📅 Wednesday, Oct 23, 2025 (Total: [DATE_HOURS]h)
  • [WORK_ITEM_ID]: [WORK_ITEM_TITLE] - [HOURS]h
```

**Verbosity Level 3 (ID, Title, Description, & Hours):**
```
📅 Monday, Oct 21, 2025 (Total: [DATE_HOURS]h)
  • [WORK_ITEM_ID]: [WORK_ITEM_TITLE] - [HOURS]h
    📝 [WORK_ITEM_DESCRIPTION]
  • [WORK_ITEM_ID]: [WORK_ITEM_TITLE] - [HOURS]h
    📝 [WORK_ITEM_DESCRIPTION]
  • [WORK_ITEM_ID]: [WORK_ITEM_TITLE] - [HOURS]h
    📝 [WORK_ITEM_DESCRIPTION]

📅 Tuesday, Oct 22, 2025 (Total: [DATE_HOURS]h)
  • [WORK_ITEM_ID]: [WORK_ITEM_TITLE] - [HOURS]h
    📝 [WORK_ITEM_DESCRIPTION]
  • [WORK_ITEM_ID]: [WORK_ITEM_TITLE] - [HOURS]h
    📝 [WORK_ITEM_DESCRIPTION]

📅 Wednesday, Oct 23, 2025 (Total: [DATE_HOURS]h)
  • [WORK_ITEM_ID]: [WORK_ITEM_TITLE] - [HOURS]h
    📝 [WORK_ITEM_DESCRIPTION]
```

---

**"By date with hierarchy" Grouping Mode Formats:**

When user selects "By date with hierarchy" grouping, work items are organized by date first, then hierarchical tree structure within each date.

**Verbosity Level 1 (ID & Hours Only):**
```
📅 Monday, Oct 21, 2025 (Total: [DATE_HOURS]h)
  📦 Feature [FEATURE_ID] (Total: [FEATURE_HOURS]h)
    📋 Story [STORY_ID] (Total: [STORY_HOURS]h)
      ✓ Task [TASK_ID]: [TASK_HOURS]h
      ✓ Bug [BUG_ID]: [BUG_HOURS]h
  🔍 No Parent
    ✓ Task [TASK_ID]: [TASK_HOURS]h

📅 Tuesday, Oct 22, 2025 (Total: [DATE_HOURS]h)
  📦 Feature [FEATURE_ID] (Total: [FEATURE_HOURS]h)
    📋 Story [STORY_ID] (Total: [STORY_HOURS]h)
      ✓ Task [TASK_ID]: [TASK_HOURS]h

📅 Wednesday, Oct 23, 2025 (Total: [DATE_HOURS]h)
  📦 Feature [FEATURE_ID] (Total: [FEATURE_HOURS]h)
    📋 Story [STORY_ID] (Total: [STORY_HOURS]h)
      ✓ Task [TASK_ID]: [TASK_HOURS]h
      ✓ Task [TASK_ID]: [TASK_HOURS]h
```

**Verbosity Level 2 (ID, Title, & Hours):**
```
📅 Monday, Oct 21, 2025 (Total: [DATE_HOURS]h)
  📦 Feature [FEATURE_ID]: [FEATURE_TITLE] (Total: [FEATURE_HOURS]h)
    📋 Story [STORY_ID]: [STORY_TITLE] (Total: [STORY_HOURS]h)
      ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h
      ✓ Bug [BUG_ID]: [BUG_TITLE] - [BUG_HOURS]h
  🔍 No Parent
    ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h

📅 Tuesday, Oct 22, 2025 (Total: [DATE_HOURS]h)
  📦 Feature [FEATURE_ID]: [FEATURE_TITLE] (Total: [FEATURE_HOURS]h)
    📋 Story [STORY_ID]: [STORY_TITLE] (Total: [STORY_HOURS]h)
      ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h

📅 Wednesday, Oct 23, 2025 (Total: [DATE_HOURS]h)
  📦 Feature [FEATURE_ID]: [FEATURE_TITLE] (Total: [FEATURE_HOURS]h)
    📋 Story [STORY_ID]: [STORY_TITLE] (Total: [STORY_HOURS]h)
      ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h
      ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h
```

**Verbosity Level 3 (ID, Title, Description, & Hours):**
```
📅 Monday, Oct 21, 2025 (Total: [DATE_HOURS]h)
  📦 Feature [FEATURE_ID]: [FEATURE_TITLE] (Total: [FEATURE_HOURS]h)
    📋 Story [STORY_ID]: [STORY_TITLE] (Total: [STORY_HOURS]h)
      ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h
        📝 [TASK_DESCRIPTION]
      ✓ Bug [BUG_ID]: [BUG_TITLE] - [BUG_HOURS]h
        📝 [BUG_DESCRIPTION]
  🔍 No Parent
    ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h
      📝 [TASK_DESCRIPTION]

📅 Tuesday, Oct 22, 2025 (Total: [DATE_HOURS]h)
  📦 Feature [FEATURE_ID]: [FEATURE_TITLE] (Total: [FEATURE_HOURS]h)
    📋 Story [STORY_ID]: [STORY_TITLE] (Total: [STORY_HOURS]h)
      ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h
        📝 [TASK_DESCRIPTION]

📅 Wednesday, Oct 23, 2025 (Total: [DATE_HOURS]h)
  📦 Feature [FEATURE_ID]: [FEATURE_TITLE] (Total: [FEATURE_HOURS]h)
    📋 Story [STORY_ID]: [STORY_TITLE] (Total: [STORY_HOURS]h)
      ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h
        📝 [TASK_DESCRIPTION]
      ✓ Task [TASK_ID]: [TASK_TITLE] - [TASK_HOURS]h
        📝 [TASK_DESCRIPTION]
```

---

**Work Item Type Icons:**
- Feature: 📦
- User Story: 📋
- Task: ✓
- Bug: 🐛
- Issue: ⚠️
- Other types: •

**Description Formatting (Level 3 only):**
- Strip HTML tags from description field
- Limit description to first 100 characters
- Add "..." if truncated
- Indent description with 6 spaces for proper tree alignment

**Report Footer:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⏱️  Total Hours: [TOTAL_HOURS]
📊 Work Items: [WORK_ITEM_COUNT] ([FEATURE_COUNT] Features, [STORY_COUNT] Stories, [TASK_COUNT] Tasks/Bugs/Issues)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💡 Tip: [FILTER_TIP]
   Only work items with logged "Completed Work" hours are included.
```

**Filter Tip Text** (based on task filter type from Step 3):
- Option 1 (Closed only): "This report shows work items that were closed during the specified period."
- Option 2 (Worked on only): "This report shows work items that were worked on (but not closed) during the specified period."
- Option 3 (Both): "This report shows all work items (both closed and worked on) during the specified period."

Replace placeholders:
- `[TOTAL_HOURS]` → Total hours across all work items
- `[WORK_ITEM_COUNT]` → Total count of all work items
- `[FEATURE_COUNT]` → Count of Features
- `[STORY_COUNT]` → Count of User Stories
- `[TASK_COUNT]` → Count of Tasks, Bugs, Issues combined

**IMPORTANT**:
- Use proper indentation for tree hierarchy (2 spaces per level)
- Show totals at each parent level (sum of children) for hierarchy-based grouping
- Use appropriate icons for different work item types
- For verbosity level 3, strip HTML and truncate descriptions
- Display "No Parent" section only if there are orphaned work items (for hierarchy-based grouping)
- Format hours with 1 decimal place (e.g., "2.5h" not "2.50h")
- If no work items found, display a friendly message indicating no hours were logged during the period
- **Select the correct display format** based on grouping mode:
  - "By hierarchy": Use traditional tree structure (Feature > Story > Task)
  - "By date": Use date headers with flat work item lists (no hierarchy)
  - "By date with hierarchy": Use date headers with tree structure within each date
- For date-based grouping modes, format dates as "Day of Week, Month DD, YYYY" (e.g., "Monday, Oct 21, 2025")
- For date-based grouping modes, show total hours for each date

### Important Constraints

**DO NOT:**
- Proceed without Azure DevOps configuration in CLAUDE.md
- Use bash commands for file operations
- Use AskUserQuestion for free-form input steps (Steps 1a and 2a should be simple text output)
- Ask questions one at a time when they can be combined (use the 2-step approach described in Phase 2)
- Include work items with Completed Work = 0 or null
- Use a hardcoded WIQL query (must build dynamically based on user choices)
- Use ClosedDate when user selected "worked on only" filter (must use ChangedDate)
- Use placeholders or empty values in the report
- Ignore the task filter type or date field selections from Phase 2
- Second-guess or reinterpret the current date from `<env>` - trust it exactly as ISO 8601 format (YYYY-MM-DD where month is middle number)

**DO:**
- Use **Read tool** to check CLAUDE.md for Azure DevOps configuration
- Use **AskUserQuestion tool** exactly twice in Phase 2:
  - Step 1: Combine 4 report configuration questions (week definition, time period, task filter type, date field)
  - Step 2: Combine 3 display option questions (verbosity level, grouping mode, user selection)
- Use simple text output for conditional free-form inputs (Steps 1a and 2a)
- Wait for user's response after each step before proceeding
- Trust the current date from `<env>` exactly as provided in ISO 8601 format (YYYY-MM-DD)
- Remember: In ISO format, month is the MIDDLE number (01=Jan, 02=Feb, 03=Mar, 04=Apr, 05=May, 06=Jun, 07=Jul, 08=Aug, 09=Sep, 10=Oct, 11=Nov, 12=Dec)
- Calculate week dates based on the correct current date without reinterpreting the format
- Build WIQL query dynamically based on task filter type and date field selections:
  - Closed only: Use State = 'Closed' + user's date field choice
  - Worked on only: Use State <> 'Closed' + ChangedDate (override date field choice)
  - Both: No state filter + user's date field choice
- Use correct Azure DevOps field names in WIQL queries:
  - Closed Date field: `Microsoft.VSTS.Common.ClosedDate` (NOT `System.ClosedDate`)
  - Changed Date field: `System.ChangedDate` (standard field, but verify if query fails)
  - State field: `System.State`
  - Note: Azure DevOps field names can vary by process template; if a field fails, check error message for correct field name
- Use **wit_query** MCP tool to execute the dynamically built query
- Calculate date ranges programmatically based on user selections
- Filter work items by user assignment after query
- Always filter by CompletedWork > 0
- Organize work items based on grouping mode selection:
  - "By hierarchy": Build proper hierarchical tree structure and roll up hours from children to parents
  - "By date": Group by date with flat list (no hierarchy)
  - "By date with hierarchy": Group by date first, then build hierarchy within each date
- Handle orphaned work items under "No Parent" (for hierarchy-based grouping modes)
- Display report with appropriate verbosity level and grouping mode format
- Use proper formatting with emojis and tree structure (or flat list for "By date" mode)
- Show helpful summary statistics in footer with filter-appropriate tip text
- Format dates in user-friendly format (e.g., "Jan 13, 2025" for report header, "Monday, Oct 21, 2025" for date grouping)
- Format hours with appropriate precision

### Error Handling

If the MCP tool returns an error:
- Display the error message to the user
- **If error contains "Cannot find field"**:
  - The Azure DevOps process template may use different field names
  - Common alternatives to try:
    - For Closed Date: Try `Microsoft.VSTS.Common.ClosedDate` or `System.ClosedDate`
    - For Changed Date: Try `System.ChangedDate` or `Microsoft.VSTS.Common.ChangedDate`
  - Adjust the WIQL query with the correct field name and retry
- Suggest other possible solutions:
  - Verify Azure DevOps MCP server is configured and running
  - Check that project name is correct
  - Ensure user has permissions to query work items
  - Verify Node.js 20+ is installed for MCP server
  - Check that date range is valid
  - Ensure WIQL query syntax is correct
- Recommend running /ado-init if configuration seems incorrect

If no work items are found:
- Display a friendly message (customize based on task filter type):
  ```
  📊 Timesheet Report
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  📅 Period: [START_DATE] to [END_DATE] ([WEEK_TYPE])
  👤 User: [USER_NAME]
  🔍 Filter: [FILTER_DESCRIPTION]
  📅 Date Field: [DATE_FIELD_NAME]
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  No work items with logged hours were found for this period.

  This could mean:
  - No work items matched the filter criteria during this time period
  - No work items had "Completed Work" hours logged
  - The specified user had no work items assigned

  💡 Tip: Make sure to set "Completed Work" hours on tasks before
     closing them, or use /ado-log-story-work to log hours.
  ```

  Replace placeholders:
  - `[FILTER_DESCRIPTION]`: "Closed only" / "Worked on only" / "Both closed and worked on"
  - `[DATE_FIELD_NAME]`: "Closed Date" / "Changed Date"

If date validation fails (for specific week option):
- Display helpful error message:
  ```
  ❌ Invalid end date for week definition

  For [WEEK_TYPE] weeks, the end date must be a [EXPECTED_DAY].
  You provided: [PROVIDED_DATE] which is a [ACTUAL_DAY].

  Please provide an end date that falls on a [EXPECTED_DAY].
  ```

### Additional Features

**Date Formatting:**
- Input dates: YYYY-MM-DD (e.g., 2025-01-17)
- Display dates: Month DD, YYYY (e.g., Jan 17, 2025)

**Hour Formatting:**
- Display with 1 decimal place if needed (e.g., "2.5h")
- Round to 1 decimal place for display
- Sum all hours accurately before rounding for display

**Work Item Type Handling:**
- Support all Azure DevOps work item types (Task, Bug, Issue, etc.)
- Use appropriate icons for common types
- Fall back to bullet point for unknown types
- Treat all leaf nodes as potential hour sources (not just Tasks)

**Empty States:**
- Handle work items with no description gracefully
- Handle work items with no parent properly
- Handle users with no assigned work items
- Display helpful messages for all empty states
