#!/bin/bash

# Function to manage IAM

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

# Function to deploy Apache Web server on CentOS
install_apache_centos() {
    sudo yum install -y httpd
    sudo systemctl start httpd
    sudo systemctl enable httpd
}

# Function to deploy Apache Web server on Ubuntu
install_apache_ubuntu() {
    sudo apt-get update
    sudo apt-get install -y apache2
    sudo systemctl start apache2
    sudo systemctl enable apache2
}

# Main function for IAM management
main_iam() {
    create_group
    attach_policy
    add_users_to_group
}

# Main function for Apache Web server deployment
main_apache() {
    # Check if the system is CentOS
    if [ -f "/etc/centos-release" ]; then
        install_apache_centos
    # Check if the system is Ubuntu
    elif [ -f "/etc/lsb-release" ]; then
        install_apache_ubuntu
    else
        echo "Unsupported distribution. This script supports CentOS and Ubuntu only."
        exit 1
    fi
}

# Execute main function based on user input
echo "Enter 'IAM' to manage IAM or 'Apache' to deploy Apache Web server:"
read input

case $input in
    "IAM")
        main_iam
        ;;
    "Apache")
        main_apache
        ;;
    *)
        echo "Invalid input. Please enter 'IAM' or 'Apache'."
        exit 1
        ;;
esac
