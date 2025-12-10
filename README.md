

If you get stuck with an issue around roles, and have to nuke them, run this from your console

```bash
for ROLE in $(aws iam list-roles \
  --query "Roles[?starts_with(RoleName, 'AmazonBedrockAgentCoreSDK')].RoleName" \
  --output text); do
  echo "🔪 Cleaning up role: $ROLE"

  # Delete inline policies
  for P in $(aws iam list-role-policies --role-name "$ROLE" --query "PolicyNames[]" --output text); do
    echo "  - Deleting inline policy: $P"
    aws iam delete-role-policy --role-name "$ROLE" --policy-name "$P"
  done

  # Detach managed policies
  for ARN in $(aws iam list-attached-role-policies --role-name "$ROLE" --query "AttachedPolicies[].PolicyArn" --output text); do
    echo "  - Detaching managed policy: $ARN"
    aws iam detach-role-policy --role-name "$ROLE" --policy-arn "$ARN"
  done

  # Delete the role
  echo "  - Deleting role: $ROLE"
  aws iam delete-role --role-name "$ROLE"
done
```

also clean up the local config
```bash
rm -f .bedrock_agentcore.yaml
```

