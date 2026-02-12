To view the raw Markdown syntax, click **Raw** in the upper‑right corner.

This example illustrates my ability to work effectively in markdown.

# **How to Authenticate with the GitHub API Using a Personal Access Token**

The GitHub REST API allows developers to interact with GitHub programmatically to retrieve data, automate workflows, and integrating GitHub features into applications. Many API endpoints require authentication, and the most common method is using a **Personal Access Token (PAT)**.

This guide includes information on how to generate a PAT and use it to make authenticated requests to the GitHub API using `curl`.

## **Prerequisites**

Before you begin, make sure you have:

- A GitHub account
    
- Basic familiarity with the command line
    
- `curl` installed on your system
    
- (Optional) A text editor or terminal that supports environment variables

## **1. Generate a Personal Access Token**

A Personal Access Token acts like a password for accessing the GitHub API. You can create one from your GitHub settings.

### **Steps**

1. Sign in to GitHub.
    
2. Navigate to **Settings → Developer settings → Personal access tokens**.
    
3. Select **Tokens (classic)** or **Fine-grained tokens**.
    
4. Click **Generate new token**.
    
5. Give your token a descriptive name.
    
6. Select the scopes you need. For this tutorial, the following are sufficient:
    
    - `read:user`
        
    - `repo` (optional, only if you want to access private repositories)
        
7. Generate the token and copy it somewhere secure.
    

> **Important:** Treat your PAT like a password. Never share it or commit it to version control.

## **2. Store Your Token Securely (Recommended)**

Instead of pasting your token directly into commands, store it in an environment variable.

### **macOS/Linux**

bash
```
export GITHUB_TOKEN="your_token_here"
```

### **Windows (PowerShell)**

powershell
```
setx GITHUB_TOKEN "your_token_here"
```

You can now reference `$GITHUB_TOKEN` (or `%GITHUB_TOKEN%` on Windows) in your commands.

## **3. Make an Authenticated Request**

Let’s start with a simple request to retrieve information about the authenticated user.
### **Example: Get Your User Profile**

bash
```
curl -H "Authorization: Bearer $GITHUB_TOKEN" \
     https://api.github.com/user
```

### **Sample Response (trimmed)**

json
```
{
  "login": "your-username",
  "id": 1234567,
  "public_repos": 42,
  "followers": 18,
  "created_at": "2018-05-10T14:32:00Z"
}
```
## **4. List Your Repositories**

Once authenticated, you can access your repositories — including private ones if your token has the correct scopes.

bash
```
curl -H "Authorization: Bearer $GITHUB_TOKEN" \
     https://api.github.com/user/repos
```
### **Optional Query Parameters**

| Parameter  | Description                                 |
| ---------- | ------------------------------------------- |
| visibility | `all`, `public`, or `private`               |
| sort       | `created`, `updated`, `pushed`, `full_name` |
| direction  | `asc` or `desc`                             |

Example:

bash
```
curl -H "Authorization: Bearer $GITHUB_TOKEN" \
     "https://api.github.com/user/repos?visibility=private&sort=updated"
```

## **5. Understanding the Authorization Header**

GitHub supports multiple authentication formats, but the recommended one is:

http
```
Authorization: Bearer <token>
```

Older documentation may show:

http
```
Authorization: token <token>
```

Both work, but **Bearer** is the modern standard and aligns with other API authentication patterns.

## **6. Common Errors and How to Fix Them**

### **401 Unauthorized**

**Cause:** Missing or invalid token **Fix:**

- Ensure the token is included in the header
    
- Verify the token hasn’t expired
    
- Confirm you copied the token correctly

### **403 Forbidden**

**Cause:**

- Token lacks required scopes
    
- You’ve hit a rate limit


**Fix:**

- Regenerate the token with additional scopes
    
- Wait for rate limits to reset

### **404 Not Found**

**Cause:** Incorrect endpoint or missing permissions **Fix:**

- Double‑check the URL
    
- Ensure your token has access to the resource

## **7. Best Practices for Token Security**

- **Never** hard‑code tokens in scripts or applications
    
- Use environment variables or secret‑management tools
    
- Add `.env` files to `.gitignore`
    
- Rotate tokens regularly
    
- Delete unused tokens

## **8. Optional: Using a REST Client**

If you prefer a graphical interface, tools like Postman, Insomnia, or Bruno make it easy to test API requests.

### **Basic steps**

1. Create a new request
    
2. Set the method (e.g., GET)
    
3. Add the URL (e.g., `https://api.github.com/user`)
    
4. Add a header:
    
    - **Key:** `Authorization`
        
    - **Value:** `Bearer <your_token>`
        
5. Send the request and inspect the response

## **Conclusion**

You’ve now generated a Personal Access Token and used it to authenticate with the GitHub API. With this foundation, you can explore more advanced topics such as pagination, filtering, repository management, or even GitHub’s GraphQL API.

This workflow mirrors real‑world developer documentation and demonstrates a practical, secure approach to API authentication.