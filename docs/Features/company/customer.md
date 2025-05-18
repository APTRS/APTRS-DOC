# Customer Management in APTRS

Customers in APTRS are external users who belong to external (non-internal) companies. You can manage these users from the Customer page, where external users are added and tracked, and they are included in reports as customer users.

![Customer Management Page](https://raw.githubusercontent.com/APTRS/APTRS-Changelog/refs/heads/main/images/customer.png)

## Understanding Customer Users

Customer users provide your clients with secure access to APTRS, allowing them to:

- View their projects and assessments
- Download finalized reports
- Track project status and timelines
- Access historical vulnerability data
- Communicate with your team

!!! note "Prerequisite: Email Configuration"
    The customer invitation process requires a properly configured email (SMTP) server in APTRS. Without email configuration, invitations cannot be sent. See the [Environment Configuration](../../installation/env.md) documentation for details on setting up email services.


## Adding Customers

When adding a customer, you only need to provide:

- **Email**: The customer's email address (must be valid to receive invitation)
- **Company**: The external company they belong to

Unlike previous versions, you no longer need to manually set a password when creating customer accounts. Instead, APTRS uses a secure invitation process.

### The Customer Invitation Process

1. An administrator or user with appropriate permissions creates a new customer account
2. APTRS automatically generates an invitation with a secure registration link
3. The system sends this invitation to the customer's email address
4. The customer receives the email and clicks the registration link
5. The customer sets their own password and completes account setup
6. After accepting the invitation, the customer gains access to their company's APTRS portal

!!! info "Invitation Link Validity"
    The invitation link is **valid for 24 hours** and can only be used once. If the link expires or has already been used, a new invitation must be sent.

!!! warning "Email Configuration Required"
    For invitations to work properly, your APTRS instance must have SMTP server settings correctly configured in the environment variables. Without this, invitation emails cannot be sent.

### Managing Customer Invitations


#### Resending Invitations

If a customer has not accepted their invitation (the status shows "Pending"), you can resend the invitation:

1. Navigate to the Customer Management page
2. Find the customer with pending status
3. Click the "Resend Invitation" button
4. The system will generate a new invitation link and send it to the customer

This is useful when:

- The original invitation email was missed or filtered as spam
- The invitation link has expired (after 24 hours)
- The customer needs a reminder to complete their registration
- The previous link was already used but registration was not completed

## Customer Portal Access

Once customers have accepted their invitations and set up their accounts, they gain access to the APTRS customer portal. This secure portal provides:

1. **Project Dashboard**: View all projects associated with their company
2. **Report Access**: Download completed assessment reports
3. **Project Status**: Track the progress of ongoing assessments
4. **Communication**: Direct messaging with your team **[TBD]**

### Access Limitations

Customer users can only access information related to their own company. They cannot:

- View projects or reports from other companies
- Access internal APTRS administration features
- Modify assessment data or findings

## Best Practices for Customer Management

### Creating Customer Accounts

- Create customer accounts before starting their projects
- Verify email addresses carefully to ensure invitations reach the correct recipients
- Add informative notes to customer profiles for internal reference
- Group customers properly by their respective companies

### Communication Management

- Inform customers via traditional channels that they will receive an APTRS invitation
- Provide instructions on what to do with the invitation
- Suggest checking spam folders if they don't receive the invitation
- Set expectations about what they'll be able to access in the portal

### Security Considerations

- Review and adjust customer permissions as needed
- Periodically audit customer account activity
- Disable accounts for customers who no longer require access

## Troubleshooting Customer Invitations

### Common Issues and Solutions

| Issue | Possible Cause | Solution |
|-------|---------------|----------|
| Invitation not received | Incorrect email address | Verify and update email, then resend |
| | Email filtered as spam | Ask customer to check spam/junk folders |
| | SMTP configuration issue | Check APTRS email configuration |
| Invitation link expired | 24-hour validity period passed | Resend invitation |
| | Link already used once | Resend invitation if needed |
| Customer can't set password | Password requirements not met | Advise on password requirements |
| | Technical browser issue | Suggest using different browser |
| Access issues after setup | Permission configuration | Review customer permissions |

!!! tip "Testing Invitations"
    Before deploying to production, test the invitation process with an internal email address to ensure that the entire workflow functions correctly.

### SMTP Configuration Validation

If customers aren't receiving invitations, verify your SMTP configuration:

1. Check the environment variables for email settings
2. Test email functionality through the APTRS administration panel
3. Review server logs for email sending errors
4. Ensure your email provider allows automated messages