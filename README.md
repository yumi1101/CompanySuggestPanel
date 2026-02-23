# CompanySuggestPanel

Salesforce Lightning Web Component (LWC) for company name suggestions using OpenCorporates API integration.

## Overview

CompanySuggestPanel is a Lightning Web Component that provides real-time company name suggestions as users type. It integrates with the OpenCorporates API to search for company information across multiple jurisdictions.

### Features

- **Real-time Search**: Debounced input handler for efficient API calls
- **Multi-jurisdiction Support**: Search across different jurisdictions
- **Company Information Display**: Shows company name, jurisdiction code, company number, and status
- **Error Handling**: Comprehensive error messages and user feedback
- **Debug Mode**: Built-in debug logging for troubleshooting
- **Mock Mode**: Test mode support without API token

## Components

### Lightning Web Components (LWC)

- `companySuggestPanel` - Main search and suggestion component
  - Real-time company search functionality
  - Candidate list display with status information
  - Event dispatch for company selection

### Apex Classes

- `CompanySuggestService` - Service class for company search
  - `searchCompanies()` - Callout to OpenCorporates API
  - `CompanyCandidate` DTO for API response mapping

- `CompanySuggestServiceTest` - Unit tests
  - Mock HTTP response testing

## Configuration

### Prerequisites

- Salesforce DX CLI installed
- Org alias configured (default: `yumiorg`)
- OpenCorporates API token (optional for mock mode)

### API Token Setup

To use the actual OpenCorporates API:

1. Create a Custom Label `OpenCorporatesApiToken` with your API token
2. Configure the Named Credential `OpenCorporates` in your Salesforce org
3. The component will automatically use the token for API calls

### Mock Mode

For development/testing without an API token:
- The component detects when no token is available
- Returns mock data for testing purposes
- Console logging for debugging

## Development

### Project Structure

```
.
├── config/
│   └── project-scratch-def.json
├── force-app/
│   └── main/
│       └── default/
│           ├── classes/
│           │   ├── CompanySuggestService.cls
│           │   ├── CompanySuggestService.cls-meta.xml
│           │   ├── CompanySuggestServiceTest.cls
│           │   └── CompanySuggestServiceTest.cls-meta.xml
│           └── lwc/
│               └── companySuggestPanel/
│                   ├── companySuggestPanel.js
│                   ├── companySuggestPanel.html
│                   ├── companySuggestPanel.js-meta.xml
│                   └── __tests__/
│                       └── companySuggestPanel.test.js
├── sfdx-project.json
└── README.md
```

### Build and Deploy

```bash
# Create a scratch org
sfdx force:org:create -f config/project-scratch-def.json -a CompanySuggestPanel

# Deploy source code
sfdx project:deploy:start --source-dir force-app/main/default --target-org CompanySuggestPanel

# Run tests
sfdx force:apex:test:run -u CompanySuggestPanel
```

## Usage

Add the component to a Lightning page:

```xml
<c-company-suggest-panel></c-company-suggest-panel>
```

Listen for the `companyselect` event:

```javascript
const panel = document.querySelector('c-company-suggest-panel');
panel.addEventListener('companyselect', (event) => {
  const selectedCompany = event.detail.company;
  console.log(selectedCompany);
});
```

## Testing

Unit tests are included in `CompanySuggestServiceTest.cls`:

```bash
sfdx force:apex:test:run -u <target-org>
```

## Debug Features

- **debugMsg**: Display component initialization status on screen
- **Console Logging**: JavaScript console logs for tracking lifecycle
- **Apex Debug Logs**: Logging in Apex classes for API interactions

## License

MIT

## Related Documentation

- [Salesforce Lightning Web Components](https://developer.salesforce.com/docs/component-library/overview/components)
- [OpenCorporates API Documentation](https://api.opencorporates.com/documentation)
