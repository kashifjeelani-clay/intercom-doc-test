# Send Table Data
Send data between tables to create simple multi-table setups.

Send table data in Clay lets you route records between tables, making multi-table setups simple to manage and intuitive to use.

**Note:** Send Table Data and Write to Other Table work the same way — you can use either one interchangeably. If you're already used to Write to Other Table, feel free to keep using it for new multi-table workflows going forward.

---

## When to use Lookup Rows vs. Send Table Data

Both Lookup Rows and Send Table Data move information between tables, but they work in opposite directions and serve different purposes.

**Use Lookup Rows when...**

Lookup Rows pulls data from another table into your current table based on matching criteria. Use it when your table has more than 10,000 rows — it's optimized for large datasets and performs significantly better than Send Table Data at that scale.

- Your source table has more than 10,000 rows
- You need to enrich your current table with data that already exists in another table
- You want to check if a record exists in another table without modifying anything
- You need to count or aggregate rows that share a trait (e.g., how many people work at each company)
- You want to reference data from a central table (like a pricing list, messaging library, or Do Not Contact list)
- You're working with static reference data that multiple tables need to access

**Use Send Table Data when...**

Send Table Data pushes data from your current table into another table. It creates or updates rows in the destination table. It's best suited for smaller tables (under 10,000 rows).

- You need to route or segment your data into different tables based on logic or filters
- You want to flatten lists into individual rows (e.g., turn a list of 5 people into 5 separate rows)
- You need to merge data from several tables into one consolidated table
- You want to separate concerns across multiple tables (e.g., companies in one table, people in another)
- You're building multi-stage workflows where each table handles a specific step in your process

---

## Sending table data

To send data from one table to another:

1. While in a table, click **+ Add column** at the end of the column headers and select **Send table data** (listed under Exports). You can also reach it by opening the Exports tab in the command center sidebar.
2. Select the destination table.
3. Choose the method:
   - **Send row:** Choose which columns to send as a row to the other table.
   - **Send row for each item in a list**
4. Select data to send over.
5. Click **Save**.

---

## Using Send row

Send row sends each row as-is to another table. Use it to filter or segment data, or to separate logic across multiple tables. It transfers specific columns, keeping your data aligned and reducing manual entry.

Additionally you can:

- Send nested data from the parent table.
- Map any selected column to a specific existing column in the destination table.

**Note:** When you first send a row, it creates a new row in the destination table. For subsequent sends, it updates that same row. You can turn this off to always create a new row via the **Update existing rows on re-run** setting.

---

## Using Send row for each item in a list

Each cell can hold a list of items — like a list of people found at a company, or a list of job postings returned by Find Active Job Openings. To turn each item in that list into its own row in another table, use **Send row for each item in a list**.

This method sends a maximum of **50 items per row per run**. If the list has more than 50 items, only the first 50 will be sent — there is no setting to increase this limit.

**Selecting the right list field manually**

When configuring the list field, select a specific indexed element from within the list (e.g., `People.0`) rather than the whole array. Selecting the entire array will cause a "Please add a valid list." error because Clay expects a reference to a single element, not the collection itself.

If the column holds a stringified JSON array — for example, a text value that looks like `[{"name": "Alice"}, {"name": "Bob"}]` — Clay automatically detects this and can iterate over it without any extra steps needed.

**Tip: Use "Take action on list" to set this up automatically**

The easiest way to configure Send row for each item in a list is to use the **Take action on list** shortcut from the cell details panel:

1. Click on a cell in the column whose list you want to flatten.
2. In the Cell details panel, hover over the list section to reveal the **Take action on list** button.
3. Select **Write each item to new row in other table**.

This opens the Send Table Data configuration with the correct list field already pre-populated.

---

## Advanced settings

**Update existing rows on re-run**

When re-running, updates the matching row in the destination table instead of creating a new one.

**Auto-extract new columns**

Automatically creates new columns in the destination table for any that don't already exist.

**Auto-map existing columns**

Automatically maps incoming fields to existing destination columns that share the same name.

---

## Mapping table data in the destination table

When you send data to a destination table, it appears in the leftmost column as "rows from: [source table name]". The data is stored within this cell and needs to be extracted into individual columns to use it in your workflow.

**Note:** "✅ Sent" means all fields have been successfully extracted and are visible in the destination table — no further mapping steps are required.

To manually map data from the source column to columns in the destination table:

1. Click into the source cell.
2. Hover over any field you want to extract.
3. Click **Add to column**.
4. Choose to either create a new column or map to an existing one.

---

## Best practices & troubleshooting

- There can be a maximum of **50 tables** connected via Send Table Data across your entire workspace, including tables in other workbooks. Lookup Rows connections also count toward this limit.
- Data can be sent in any direction, including circular loops (A → B → C → A). This is useful for workflows that need to continuously sync data between tables.
- Some rows show **"Invalid send table data inputs … 'listData' … 'Required'"** while others succeed: this means the list column had no data for that specific row. Fix by adding a run condition so the action only runs when the list column has a value.
- Rows created by Send row for each item in a list don't have an automatic array-index column. To add a unique identifier per expanded row, add a formula column that uses `Math.random()` — this generates a stable unique ID for each row that persists across re-runs.
