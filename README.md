
# Moodle Quiz Slot Cleanup

<p align="center">
  <img src="logos/artcodix_logo.png" alt="Artcodix Logo" height="120"/>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <img src="logos/Logo_Technische_Hochschule_Rosenheim.svg.png" alt="Hochschule Rosenheim Logo" height="120"/>
</p>

<p align="center">
  <strong>Created by Artcodix in cooperation with Hochschule Rosenheim</strong>
</p>

---

A Jupyter notebook tool for identifying and cleaning up orphaned quiz slots in a Moodle PostgreSQL database. This tool helps maintain database integrity by removing quiz slots that have no associated question references.


## Overview

This notebook provides a comprehensive workflow to:
- Identify quiz slots without question references
- Delete orphaned quiz slots
- Fix slot and page numbering gaps
- Update quiz section references
- Maintain database integrity throughout the cleanup process

## Features

- ✅ **Safe Testing Mode**: Run in read-only mode to preview changes before applying them
- ✅ **Analysis**: Visualize quiz slot data by year and course
- ✅ **Smart Detection**: Identifies slots without question references (both regular and random questions)
- ✅ **Automatic Reordering**: Fixes gaps in slot/page numbering after deletions
- ✅ **Quiz Section Management**: Updates or deletes quiz sections as needed
- ✅ **Transaction Safety**: Proper rollback handling on errors

## Prerequisites

### Required Python Packages

```bash
pip install psycopg2-binary pandas matplotlib
```

### Database Access

- PostgreSQL database with Moodle schema
- Read-only user credentials (for testing)
- Owner user credentials (for production operations)

## Configuration

### Database Settings

The notebook uses two database users:

1. **Reader User** (Read-only, for testing):
   - Used when `isTesting = True`
   - Safe for testing and analysis
   - Cannot modify database

2. **Owner User** (Write access, for production):
   - Used when `isTesting = False`
   - Required for actual cleanup operations
   - Can delete and update records

### Testing Mode

Set the testing mode at the beginning of the notebook:

```python
isTesting = True   # Safe mode - no database changes
isTesting = False  # Production mode - applies changes
```

## Usage

### 1. Initial Setup

1. Open `QuizSlot_Cleanup.ipynb` in Jupyter
2. Update database credentials in the configuration cell
3. Set `isTesting = True` for initial run

### 2. Run Analysis

Execute cells sequentially to:
- Connect to the database
- Query quiz slots without question references
- Analyze data by year and course
- Visualize the distribution

### 3. Review Findings

The notebook will show:
- Total quiz slots without references
- Quizzes grouped by slot count
- Slots identified for deletion
- Visualization of data by year

### 4. Test Mode Execution

With `isTesting = True`, run the deletion and reordering cells to see:
- What slots would be deleted
- What sections would be updated
- What reordering would occur

### 5. Production Execution

When ready:
1. Set `isTesting = False`
2. Update database credentials to owner user
3. Re-run deletion and reordering cells
4. Review summary statistics


## Database Tables

The notebook interacts with these Moodle tables:

- `mdl_quiz_slots` - Quiz slot definitions
- `mdl_question_references` - Regular question references
- `mdl_question_set_references` - Random question references
- `mdl_quiz_sections` - Quiz section definitions
- `mdl_quiz` - Quiz metadata
- `mdl_course` - Course information
- `mdl_course_modules` - Course module data
- `mdl_context` - Context information

## Troubleshooting

### Connection Issues
- Verify database host and port
- Check user credentials
- Ensure PostgreSQL is running

### Permission Errors
- Confirm owner user has write permissions
- Check table-level permissions
- Verify role assignments

### Data Type Errors
- The notebook handles numpy.int64 to Python int conversions
- Parameterized queries prevent SQL injection

## Best Practices

1. **Create a Venv**: Create a virtual environment and install the packages
2. **Always test first**: Run with `isTesting = True` before production
3. **Backup database**: Create a backup before running in production mode
4. **Review exclusions**: Update `quiz_ids_with_section_references` as needed
5. **Monitor output**: Check logs for any error messages
6. **Verify results**: Query the database after cleanup to confirm changes

## Limitations

- Only works with PostgreSQL databases
- Requires specific Moodle table structure
- Does not handle concurrent modifications
- Assumes standard Moodle schema

## Contributing

When modifying this notebook:
1. Test thoroughly with `isTesting = True`
2. Document new functions
3. Update this README
4. Maintain transaction safety

---

**Last Updated**: November 2025  
**Moodle Version Compatibility**: Tested with Moodle 4.5 schema
