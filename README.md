# Setting up AWS EKS MCP Server with Cursor on macOS

This guide will walk you through setting up the AWS EKS MCP (Model Context Protocol) server with Cursor IDE on macOS. The MCP server enables Cursor to interact with your AWS EKS clusters directly, providing powerful Kubernetes management capabilities.

## Prerequisites

Before you begin, ensure you have the following installed on your macOS system:

### 1. Required Software
- **macOS 10.15 (Catalina) or later**
- **Cursor IDE** - Download from [cursor.sh](https://cursor.sh)
- **Python 3.8+** - Install via [python.org](https://python.org) or Homebrew
- **Git** - For version control

### 2. AWS Prerequisites
- **AWS CLI** - Install via Homebrew or download from AWS
- **AWS Account** with appropriate permissions
- **EKS Cluster** (existing or you'll create one)
- **AWS Credentials** configured

### 3. Homebrew (Recommended)
If you don't have Homebrew installed, install it first:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## Installation Steps

### Step 1: Install Python and uv

The AWS EKS MCP server requires Python and the `uv` package manager for optimal performance.

```bash
# Install Python via Homebrew
brew install python

# Install uv package manager
curl -LsSf https://astral.sh/uv/install.sh | sh

# Restart your terminal or source your shell profile
source ~/.zshrc  # or ~/.bash_profile for bash users
```

### Step 2: Install AWS CLI

```bash
# Install AWS CLI via Homebrew
brew install awscli

# Verify installation
aws --version
```

### Step 3: Configure AWS Credentials

```bash
# Configure your AWS credentials
aws configure

# You'll be prompted for:
# - AWS Access Key ID
# - AWS Secret Access Key
# - Default region (e.g., us-west-2)
# - Default output format (json)
```

**Alternative: Use AWS SSO (Recommended for enterprise users)**
```bash
aws configure sso
```

### Step 4: Install Cursor IDE

1. Download Cursor from [cursor.sh](https://cursor.sh)
2. Install the downloaded `.dmg` file
3. Launch Cursor and complete the initial setup

### Step 5: Configure MCP Server in Cursor

1. **Open Cursor Settings:**
   - Press `Cmd + ,` (Command + Comma)
   - Or go to `Cursor > Settings`

2. **Navigate to MCP Settings:**
   - Search for "MCP" in the settings search bar
   - Or navigate to `AI > MCP Servers`

3. **Add the EKS MCP Server:**
   - Click "Add Server"
   - Use the following configuration:

```json
{
  "mcpServers": {
    "awslabs.eks-mcp-server": {
      "autoApprove": [],
      "disabled": false,
      "command": "uvx",
      "args": [
        "awslabs.eks-mcp-server@latest",
        "--allow-write",
        "--allow-sensitive-data-access"
      ],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "transportType": "stdio"
    }
  }
}
```

### Step 6: Verify Installation

1. **Restart Cursor** after adding the MCP server
2. **Open Command Palette** (`Cmd + Shift + P`)
3. **Type "MCP"** to see available MCP commands
4. **Test the connection** by running an MCP command

## Configuration Options

### MCP Server Flags

The EKS MCP server supports several configuration flags:

- `--allow-write`: Enables write operations (create, update, delete resources)
- `--allow-sensitive-data-access`: Allows access to sensitive data like logs and secrets
- `--log-level`: Set logging level (DEBUG, INFO, WARNING, ERROR)

### Environment Variables

You can configure additional environment variables:

```json
"env": {
  "FASTMCP_LOG_LEVEL": "ERROR",
  "AWS_DEFAULT_REGION": "us-west-2",
  "AWS_PROFILE": "your-profile-name"
}
```

## Usage Examples

### 1. List EKS Clusters
```bash
# In Cursor's terminal or via MCP commands
# The MCP server will automatically detect and list your EKS clusters
```

### 2. Manage Kubernetes Resources
```bash
# The MCP server provides commands for:
# - Listing pods, services, deployments
# - Getting logs from pods
# - Managing resources (create, update, delete)
# - Monitoring cluster metrics
```

### 3. Troubleshooting
```bash
# Check MCP server status
# View logs and events
# Monitor resource health
```

## Troubleshooting

### Common Issues and Solutions

#### 1. MCP Server Not Starting
**Problem:** The MCP server fails to start or shows connection errors.

**Solutions:**
- Verify Python and uv are installed: `uv --version`
- Check AWS credentials: `aws sts get-caller-identity`
- Ensure the MCP configuration is correct in Cursor settings
- Check Cursor's developer console for error messages

#### 2. Permission Denied Errors
**Problem:** Getting permission errors when trying to access EKS resources.

**Solutions:**
- Verify your AWS user/role has EKS permissions
- Check if your EKS cluster is accessible from your current AWS profile
- Ensure the cluster's security groups allow your IP address

#### 3. uv Command Not Found
**Problem:** Cursor can't find the `uv` command.

**Solutions:**
- Ensure uv is in your PATH: `which uv`
- Restart Cursor after installing uv
- Use absolute path to uv in MCP configuration if needed

#### 4. AWS Region Issues
**Problem:** MCP server can't find your EKS cluster.

**Solutions:**
- Verify the correct AWS region is configured
- Check if your EKS cluster exists in the configured region
- Use `aws eks list-clusters` to verify cluster names

### Debug Mode

To enable debug logging, modify the MCP configuration:

```json
"env": {
  "FASTMCP_LOG_LEVEL": "DEBUG"
}
```

## Security Considerations

### 1. IAM Permissions
- Use the principle of least privilege
- Only grant necessary EKS permissions
- Consider using AWS SSO for enterprise environments

### 2. Network Security
- Ensure your EKS cluster's security groups are properly configured
- Use private subnets when possible
- Consider using AWS VPN or Direct Connect for secure access

### 3. Credential Management
- Never commit AWS credentials to version control
- Use AWS IAM roles when possible
- Rotate access keys regularly

## Support and Resources

### Official Documentation
- [AWS EKS Documentation](https://docs.aws.amazon.com/eks/)
- [Cursor Documentation](https://cursor.sh/docs)
- [MCP Protocol Specification](https://modelcontextprotocol.io/)

### Community Resources
- [AWS EKS Community](https://github.com/aws/eks-charts)
- [Cursor Discord](https://discord.gg/cursor)
- [MCP Community](https://github.com/modelcontextprotocol)

## License

This guide is provided as-is for educational purposes. Please refer to the respective licenses for AWS EKS, Cursor IDE, and the MCP server.

---

**Note:** This guide assumes you have basic familiarity with AWS, Kubernetes, and command-line tools. If you're new to these technologies, consider reviewing the official documentation first. 
