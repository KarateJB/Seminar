# Database automation deploy

# 現況(Why)

- Only Operation team
- Route jobs
- Complicated environments
- Wrong version
- Error may cause down-time

# 效益

- Fast deply
- Small segment and deploy frequently

# 實作

## Version control

1. Visual Studio Database project
2. Git
3. Jenkins

## Database deploy

### Model-driven approach

- Compare Production and Develop versions

1. Build
2. Produce DACPAC file (ZIP file)
3. Powershell `SQLPACKAGE /ACTION: script /sourceFile: `

> 3rd package: [SQL Compare](https://www.red-gate.com/products/sql-development/sql-compare/)

### Change-driven approach

1. ORM tooling, such as Entity framework


# 流程管理

- Table lock:
  - PK changes
  - Release Index
- Data migration is hard
  - Create new Store Procedure, not change old ones
  - Deploy a new database and switch!




