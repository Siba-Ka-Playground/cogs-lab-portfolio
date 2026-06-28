# Changes
## Change Plan Template:
## Change ID: CHANGE-001
## Type: Normal Change
## Description: Update Nginx TLS config to prefer TLS 1.3
## Justification: TLS 1.3 is faster and more secure - align with best practice
## Execution Steps:
##   1. Backup current nginx config
##   2. Update ssl_protocols line in nginx site config
##   3. Test config with nginx -t
##   4. Reload nginx
##   5. Verify TLS 1.3 is working
## Rollback Trigger: If TLS verification fails or site becomes unreachable
## Rollback Steps:
##   1. sudo cp /etc/nginx/nginx.conf.backup /etc/nginx/nginx.conf
##   2. sudo nginx -t && sudo systemctl reload nginx
## Rollback Time: 2 minutes
## Maintenance Window: Immediate (lab environment - no customer impact)
