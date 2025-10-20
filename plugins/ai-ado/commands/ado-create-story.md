# Create Azure DevOps User Story

Interactively create a new User Story work item as a child of an existing Feature in Azure DevOps using the organization's configured conventions and guidelines.

## Instructions

This command creates a User Story work item following the organization's Azure DevOps conventions defined in CLAUDE.md.

### Phase 1: Validate Azure DevOps Configuration

Before proceeding with user story creation, verify that Azure DevOps configuration exists:

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
     - Organization name (look for text after 'specify the organization as "')
     - Project name (look for text after 'the project as "')
     - Team name (look for text after 'the team as "')
     - Area Path (look for text after 'Area Path" set to "')
     - Iteration Path (look for text after 'Iteration Path" set to "')
     - Naming convention preference (check if decimal notation is used)
   - Store these values for use in story creation
   - Proceed to Phase 2

**IMPORTANT**:
- Use **Read tool** to check CLAUDE.md - DO NOT use bash commands
- The entire command should STOP if Azure DevOps configuration is not found
- Do not prompt the user for configuration values - they must run /ado-init first

### Phase 2: Gather User Story Information

Collect user story details from the user in a sequential flow. Ask for each value and wait for the user's response before proceeding to the next.

**Step 1 - Parent Feature ID:**

Simply output the following text as your response message and STOP (DO NOT call any tools):

"What is the ID of the parent Feature for this User Story?

(This is the numeric ID of the Feature work item that will contain this story. You can find this in Azure DevOps or from the output of /ado-create-feature)"

Wait for the user's next message with the parent feature ID before proceeding.

**Step 2 - Story Title:**

After receiving the parent feature ID, simply output the following text as your response message and STOP (DO NOT call any tools):

"What is the title for this User Story?

(Based on your naming convention, use an appropriate format. For decimal notation, use: '1.1: Story Name', '2.1: Story Name', etc.)"

Wait for the user's next message with the story title before proceeding.

**Step 3 - User Persona Statement:**

After receiving the story title, simply output the following text as your response message and STOP (DO NOT call any tools):

"What is the user persona statement for this story?

(Format: 'As a [role], I want to [action] so that [benefit]'
Example: 'As a website visitor, I want to log in with my email and password so that I can access my personalized dashboard.')"

Wait for the user's next message with the persona statement before proceeding.

**Step 4 - Background Information:**

After receiving the persona statement, simply output the following text as your response message and STOP (DO NOT call any tools):

"What background information should be included for this story?

(Provide relevant context, technical details, or constraints. Use bullet points for clarity. Type 'none' to skip.)"

Wait for the user's next message with background information (or 'none') before proceeding.

**Step 5 - Acceptance Criteria:**

After receiving the background information, simply output the following text as your response message and STOP (DO NOT call any tools):

"What are the acceptance criteria for this story?

(Use 'Given, When, Then' format. Provide multiple scenarios if needed. Keep criteria concise for QA testers.
Example:
Given a registered user with valid credentials
When they enter email and password on the login page
Then they should be authenticated and redirected to dashboard)"

Wait for the user's next message with acceptance criteria before proceeding.

**Step 6 - Story Points:**

After receiving the acceptance criteria, simply output the following text as your response message and STOP (DO NOT call any tools):

"What is the Story Points estimate for this User Story?

(Use Fibonacci sequence values: 1, 2, 3, 5, 8, 13, 21, etc.)"

Wait for the user's next message with story points before proceeding.

**IMPORTANT**:
- Simply output the question text in your response message and STOP - do NOT call ANY tools
- DO NOT use the AskUserQuestion tool for any of these steps
- After outputting each question, wait for the user's next message before proceeding
- Validate that required fields are provided (not empty)
- Parent Feature ID must be a valid number

### Phase 3: Create User Story Work Item

Using the configuration from Phase 1 and the information from Phase 2, create the user story work item:

1. Build the complete description in HTML format:
   - Start with the persona statement wrapped in `<p>` tags
   - If background information was provided (not 'none'):
     - Add a blank line and `<p><strong>Background:</strong></p>`
     - Format background as bullet list with `<ul>` and `<li>` tags
   - Add a blank line and `<p><strong>Acceptance Criteria:</strong></p>`
   - Format acceptance criteria with proper line breaks using `<br/>` tags or bullet list format

2. Use the **wit_add_child_work_items** MCP tool with the following parameters:
   - `parentId`: The parent feature ID provided by user (as number, not string)
   - `project`: Use the project name from Phase 1 configuration
   - `workItemType`: "User Story"
   - `items`: Array containing a single item object with:
     - `title`: User-provided title
     - `description`: HTML-formatted description from step 1
     - `format`: "Html"
     - `areaPath`: Area Path from configuration
     - `iterationPath`: Iteration Path from configuration

3. After the user story is created, use the **wit_update_work_item** MCP tool to set Story Points:
   - `id`: The ID of the newly created user story (from the response of wit_add_child_work_items)
   - `updates`: Array containing:
     - `{"op": "add", "path": "/fields/Microsoft.VSTS.Scheduling.StoryPoints", "value": "[STORY_POINTS_VALUE]"}`

4. Wait for both MCP tool responses

**IMPORTANT**:
- Use the exact field names and paths shown above
- Ensure HTML formatting is applied to the description
- Parent ID must be converted to a number (not a string)
- Use configuration values from Phase 1, not hardcoded values
- Story Points must be set in a separate update operation after creation

### Phase 4: Display Success Message

After successful user story creation, display a comprehensive success message:

```
✓ User Story created successfully!

User Story Details:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 ID: [STORY_ID]
📋 Title: [STORY_TITLE]
👤 Parent Feature: [PARENT_FEATURE_ID]
📂 Project: [PROJECT_NAME]
📍 Area Path: [AREA_PATH]
🔄 Iteration Path: [ITERATION_PATH]
⭐ Story Points: [STORY_POINTS]
✨ State: New
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Next Steps:
1. Create Task work items under this User Story using /ado-create-task
2. Review the story in Azure DevOps web interface
3. Assign team members and refine estimates as needed

💡 To create a task for this story:
   /ado-create-task
   (You'll be prompted for parent User Story ID: [STORY_ID])

🔗 View in Azure DevOps:
   https://dev.azure.com/[ORGANIZATION]/[PROJECT]/_workitems/edit/[STORY_ID]
```

Replace placeholders with actual values from the created story and configuration.

### Important Constraints

**DO NOT:**
- Proceed without Azure DevOps configuration in CLAUDE.md
- Use bash commands for file operations
- Create the story without user-provided information
- Use placeholder or empty values
- Call tools during Steps 1-6 (questions should be simple text output)
- Include Description or Acceptance Criteria in the work item title

**DO:**
- Use **Read tool** to check CLAUDE.md for Azure DevOps configuration
- Simply output question text and STOP for Steps 1-6 (no tool calls)
- Wait for user's response after each question before proceeding
- Use **wit_add_child_work_items** MCP tool to create the story as a child of the feature
- Use **wit_update_work_item** MCP tool to set Story Points after creation
- Apply proper HTML formatting to description field
- Use configuration values from CLAUDE.md
- Show clear success message with story details
- Provide helpful next steps and links

### Error Handling

If the MCP tool returns an error:
- Display the error message to the user
- Suggest possible solutions:
  - Verify the parent Feature ID exists and is valid
  - Verify Azure DevOps MCP server is configured and running
  - Check that project and team names are correct
  - Ensure user has permissions to create work items
  - Verify Node.js 20+ is installed for MCP server
- Recommend running /ado-init if configuration seems incorrect
- If parent ID is invalid, suggest using /ado-create-feature first
