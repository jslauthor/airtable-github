# Github + Airtable integration

Hello! This project, written in [Reason](reasonml.github.io), explores some basic integration of GitHub issues with Airtable (it updates a field whenever an issue is opened or closed).

It contains:

* A very simple GitHub webhook to handle events from issues (note it will probably throw with other types of events)
* Some functions to update data in Airtable

# Build

```
npm run build
```

# Build + Watch

```
npm run start
```

# Run

```
AIRTABLE_API_KEY="your_api_key" AIRTABLE_DB_ID="your_db_id" AIRTABLE_TABLE_NAME="your_table_name" PORT="your desired port" node src/Main.bs.js
```

---



# Goals for this project

Webflow uses Airtable to host our "Master Tracking Issues" which monitor and report the progress on the various Github issues required to ship large features. This server provides all the webhooks to automate the synchronization of these Airtable databases and their related issues.

## Usage

There's a small amount of boilerplate required to link an Airtable database to Github, and yes, we wish this could be automatic, but Airtable's API doesn't have any "list all databases" operations. First, if you are creating a new Airtable base based on this template [TODO: Create and insert template], be sure to add the database id and table's name to the list of synchronized tables. Please use the admin interface to include your list.

[TODO: Screenshot pointing to table's name in database]

### Admin Interface

This webhook provides a simple configuration screen found at http://<ip:port>/admin. You will a list of databases and their synchronized table names.

### Managed Airtable Columns

Synchronization requires the Airtable Database to adhere to a set of naming conventions for column names, field types, and options.

| Column Name    | Field Type      | Options       |
| -------------- | --------------- | ------------- |
| `Issue Number` | `Number`        | --            |
| `Status`       | `Single Select` | 1, 2, 3, 4, 5 |
| `Assignee`     | `Collaborator`  | --            |

#### Issue Number

Simply paste in the issue number as an *integer*, e.g. only `12345` (no URL or hash character) and this server will start providing updates!

#### Status

The status of an issue is determined mapped to five enums that are provided to the Airtable base. These enums are then mapped to labels via a formula. Here are what the enums mean and how they are derived:

| Enum | Meaning                           | How its derived from the Github issue                        |
| ---- | --------------------------------- | ------------------------------------------------------------ |
| `1`  | The issue Hasn't Started          | The issue does not have an assignee in Github                |
| `2`  | The issue is In Progress          | The issue has an assignee with none of the labels below      |
| `3`  | The issue is in Code Review or QA | The issue has at least one of the following labels `status: need-QA`, `status: needs code-review`, or `qa: in-progress` |
| `4`  | The issue is Blocked              | The issue has the `blocked` label                            |
| `5`  | The issue is Complete             | The issue is closed in Github                                |

#### Assignee

[TODO: Is is possible to update Airtable with github assignees?]



## Meta Tracking Issues

Airtable is a fantastic lossless interface for tracking projects, but it unfortunately does not have a blog-like interface for _talking about_ the project. Please create a "Meta Tracking Issue" in Github and link to it in the Airtable document. The Meta Tracking Issue should also link to the Airtable base, too!