# Smartsheet GraphQL Schema

## Overview

This document describes a conceptual GraphQL schema for the Smartsheet REST API. Smartsheet is a SaaS work management and collaboration platform that combines spreadsheet-style sheets, project plans, Gantt charts, dashboards, forms, and workflow automation. The GraphQL schema models the core domain objects available through the Smartsheet REST API v2.0.

## Source

- API Reference: https://developers.smartsheet.com/api/smartsheet/openapi
- Documentation: https://developers.smartsheet.com/api/smartsheet/introduction
- Authentication: https://developers.smartsheet.com/api/smartsheet/guides/basics/authentication
- Base URL: https://api.smartsheet.com/2.0

## Schema Source

Schema type: Conceptual
Derived from: Smartsheet REST API v2.0 public documentation and OpenAPI specification.

## Authentication

The Smartsheet REST API supports OAuth 2.0 and Bearer access token authentication. GraphQL requests would carry the same `Authorization: Bearer <token>` header.

## Type Summary

The schema defines 62 named types organized across the following domains:

| Domain | Types |
|---|---|
| Sheets and Structure | Sheet, SheetColumn, SheetRow, Cell, CellLink, CrossSheetReference, Formula |
| Content and Files | Attachment, Discussion, Comment |
| Organization | Folder, Workspace |
| Reporting | Report, ReportRow, ReportColumn |
| Templates and Dashboards | Template, Dashboard, Widget, ChartWidget, GridWidget, ShortcutWidget, MetricWidget, ImageWidget, RichTextWidget |
| Forms | Form, FormField |
| Filtering and Rules | RowFilter, Alert, Automation, AutomationAction, Rule, Condition |
| Requests and Updates | Request, SentResult, Backup, Update, UpdateRequest, UpdateResponse |
| Users and Access | User, Group, GroupMember, Organization, Share, Contact |
| Events and Notifications | Event, PotentialAutomation, RowEmail, Send |
| System | Version, Metadata, Probe |

## Query Examples

### Fetch a Sheet with Rows and Columns

```graphql
query GetSheet($id: ID!) {
  sheet(id: $id) {
    id
    name
    columns {
      id
      title
      type
    }
    rows {
      id
      rowNumber
      cells {
        columnId
        value
        displayValue
      }
    }
  }
}
```

### List Workspaces

```graphql
query ListWorkspaces {
  workspaces {
    id
    name
    folders {
      id
      name
    }
    sheets {
      id
      name
    }
  }
}
```

### Get User and Groups

```graphql
query GetCurrentUser {
  currentUser {
    id
    email
    firstName
    lastName
    groups {
      id
      name
      members {
        email
        name
      }
    }
  }
}
```

### List Automations on a Sheet

```graphql
query GetAutomations($sheetId: ID!) {
  sheet(id: $sheetId) {
    automations {
      id
      name
      enabled
      action {
        type
        recipients {
          email
        }
      }
    }
  }
}
```

### Fetch Dashboard Widgets

```graphql
query GetDashboard($id: ID!) {
  dashboard(id: $id) {
    id
    name
    widgets {
      id
      type
      title
      xPosition
      yPosition
      width
      height
    }
  }
}
```

## Mutation Examples

### Add a Row

```graphql
mutation AddRow($sheetId: ID!, $input: RowInput!) {
  addRow(sheetId: $sheetId, input: $input) {
    id
    rowNumber
    cells {
      columnId
      value
    }
  }
}
```

### Share a Sheet

```graphql
mutation ShareSheet($sheetId: ID!, $input: ShareInput!) {
  shareSheet(sheetId: $sheetId, input: $input) {
    id
    accessLevel
    email
  }
}
```

### Create an Automation Rule

```graphql
mutation CreateAutomation($sheetId: ID!, $input: AutomationInput!) {
  createAutomation(sheetId: $sheetId, input: $input) {
    id
    name
    enabled
  }
}
```

## Notes

- This is a conceptual schema. Smartsheet does not currently expose a native GraphQL endpoint.
- The schema models the domain objects from the Smartsheet REST API v2.0.
- All IDs are represented as `ID!` (non-null) following GraphQL conventions.
- Enum values reflect Smartsheet API documentation terminology.
- Pagination follows a cursor-based pattern consistent with GraphQL best practices.
