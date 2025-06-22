# WhatsApp Business Integration for Odoo (Taqnyat.sa)

## Overview

This module integrates Odoo with WhatsApp Business API using **Taqnyat.sa** as the service provider, offering a reliable and cost-effective solution for WhatsApp messaging in the Middle East region.

## Features

### Core Functionality
- ✅ **Automated Notifications**: Send WhatsApp notifications to any department within Odoo
- ✅ **Document Transmission**: Send business documents (invoices, quotations, etc.) via WhatsApp
- ✅ **Template Management**: Create and manage WhatsApp message templates directly from Odoo
- ✅ **Two-way Communication**: Receive and respond to WhatsApp messages
- ✅ **Media Support**: Send images, documents, videos, and audio files
- ✅ **International Support**: Automatic phone number formatting for global reach

### Taqnyat.sa Advantages
- 🚀 **Simplified Setup**: Only requires a Bearer token from Taqnyat.sa portal
- 💰 **Cost Effective**: Competitive pricing with transparent billing
- 🌍 **Regional Focus**: Optimized for Middle East and Arabic markets
- 📞 **Local Support**: Arabic documentation and customer support
- ⚡ **High Reliability**: Excellent delivery rates and uptime
- 🔒 **Secure**: Enterprise-grade security and compliance

## Installation

1. **Install the Module**
   ```bash
   # Copy the module to your Odoo addons directory
   cp -r custom_addons/whatsapp /path/to/odoo/addons/
   
   # Update the module list
   odoo-bin -u whatsapp -d your_database
   ```

2. **Get Taqnyat.sa Credentials**
   - Visit [Taqnyat.sa Portal](https://portal.taqnyat.sa)
   - Create an account or login
   - Navigate to Developer → Applications
   - Create a new application for WhatsApp service
   - Copy the Bearer token

3. **Configure the Module**
   - Go to Settings → WhatsApp → WhatsApp Business Accounts
   - Create a new account
   - Enter your Bearer token
   - Test the connection
   - Synchronize templates

## Configuration

### Basic Setup

1. **WhatsApp Account Configuration**
   ```
   Settings → WhatsApp → WhatsApp Business Accounts → Create
   ```
   - **Name**: Your business account name
   - **Bearer Token**: Token from Taqnyat.sa portal
   - **Notify Users**: Users to notify for inbound messages

2. **Template Management**
   ```
   Settings → WhatsApp → Templates
   ```
   - Create message templates
   - Configure variables and components
   - Submit for approval

3. **Webhook Setup (Optional)**
   - Configure webhook URL in Taqnyat.sa portal
   - URL: `https://yourdomain.com/whatsapp/webhook/`
   - Enable message delivery reports

### Advanced Configuration

#### Phone Number Formatting
The module automatically handles international phone number formats:
- Removes `+` or `00` prefixes as required by Taqnyat.sa
- Validates phone numbers
- Supports all international formats

#### Error Handling
Comprehensive error handling with automatic retry for:
- Network issues
- Temporary API unavailability
- Rate limiting
- Authentication errors

## Usage

### Sending Messages

#### From Any Odoo Record
1. Open any record (customer, invoice, etc.)
2. Click the "WhatsApp" button in the chatter
3. Select a template
4. Fill in variables
5. Send the message

#### Programmatically
```python
# Send a template message
whatsapp_composer = self.env['whatsapp.composer'].create({
    'res_model': 'res.partner',
    'res_id': partner.id,
    'wa_template_id': template.id,
})
whatsapp_composer._send_whatsapp_template()

# Send a simple text message
message = self.env['whatsapp.message'].create({
    'mobile_number': '+966501234567',
    'body': 'Hello from Odoo!',
    'wa_account_id': account.id,
})
message._send()
```

### Template Variables

Templates support dynamic variables:
```
Hello {{1}}, your order {{2}} is ready for pickup!
```

Variables are automatically filled from the record context.

### File Attachments

Supported file types:
- **Images**: JPEG, PNG (max 5MB)
- **Documents**: PDF, DOC, XLS, etc. (max 100MB)
- **Videos**: MP4, 3GPP (max 16MB)
- **Audio**: AAC, MP4, AMR, MPEG, OGG (max 16MB)

## API Reference

### WhatsApp Account Model (`whatsapp.account`)

#### Fields
- `name`: Account name
- `token`: Taqnyat.sa Bearer token
- `active`: Account status
- `notify_user_ids`: Users to notify for inbound messages

#### Methods
- `button_test_connection()`: Test API connection
- `button_sync_whatsapp_account_templates()`: Sync templates
- `_process_messages(value)`: Process inbound messages
- `_process_taqnyat_message(notification)`: Process Taqnyat.sa webhooks

### WhatsApp Message Model (`whatsapp.message`)

#### Fields
- `mobile_number`: Recipient phone number
- `state`: Message state (outgoing, sent, delivered, read, error)
- `wa_template_id`: Associated template
- `msg_uid`: WhatsApp message ID
- `failure_reason`: Error details if failed

#### Methods
- `_send()`: Send the message
- `_process_taqnyat_status(status_data)`: Process delivery reports
- `button_resend()`: Resend failed messages

### WhatsApp Template Model (`whatsapp.template`)

#### Fields
- `name`: Template name
- `status`: Approval status
- `language`: Template language
- `template_type`: Category (marketing, utility, authentication)

## Migration from Meta API

This module provides seamless migration from Meta WhatsApp Business API:

### Automatic Migration
- Existing configurations are preserved
- Templates are automatically synced
- No data loss during migration
- Gradual migration support

### Migration Steps
1. Get Taqnyat.sa Bearer token
2. Update account configuration
3. Test connection
4. Sync templates
5. Verify functionality

See [MIGRATION_GUIDE.md](./MIGRATION_GUIDE.md) for detailed instructions.

## Troubleshooting

### Common Issues

#### Authentication Errors
```
Error: 401 - No such bot/bearer combination
```
**Solution**: Verify Bearer token in Taqnyat.sa portal

#### Template Not Found
```
Error: 132001 - Template name does not exist
```
**Solution**: Sync templates or check template status

#### Message Delivery Failed
```
Error: 402 - Invalid recipient
```
**Solution**: Verify phone number format and WhatsApp capability

### Debug Mode
Enable debug logging:
```python
import logging
logging.getLogger('odoo.addons.whatsapp').setLevel(logging.DEBUG)
```

## Support

### Documentation
- [Taqnyat.sa API Documentation](https://dev.taqnyat.sa/ar/doc/whatsapp/)
- [Migration Guide](./MIGRATION_GUIDE.md)
- [Test Documentation](./tests/)

### Contact
- **Taqnyat.sa Support**: support@taqnyat.sa
- **Portal**: https://portal.taqnyat.sa
- **Documentation**: https://dev.taqnyat.sa

## License

This module is licensed under the same terms as Odoo Community Edition.

## Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Add tests for new functionality
4. Submit a pull request

## Changelog

### Version 2.0
- Migrated to Taqnyat.sa WhatsApp Business API
- Simplified authentication with Bearer token
- Improved phone number handling
- Enhanced error handling
- Added backward compatibility
- Updated UI and documentation

### Version 1.0
- Initial release with Meta WhatsApp Business API
- Basic messaging functionality
- Template management
- Webhook support
# Whatsapp_taqnyat
