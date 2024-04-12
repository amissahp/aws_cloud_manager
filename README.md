# aws_cloud_manager

#!/bin/bash

# Array of IAM user names
users=("Prince" "Princess" "Principal" "Principe" "Princey")

# Function to create IAM group
create_group() {
    aws iam create-group --group-name admin
}

# Function to attach policy to IAM group
attach_policy() {
    aws iam attach-group-policy --group-name admin --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
}

# Function to add users to IAM group
add_users_to_group() {
    for user in "${users[@]}"; do
        aws iam add-user-to-group --user-name "$user" --group-name admin
    done
}

# Main function
main() {
    create_group
    attach_policy
    add_users_to_group
}

# Execute main function
main
