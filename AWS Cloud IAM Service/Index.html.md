<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AWS IAM & Identity Center Guide</title>

<style>
body {
    margin: 0;
    font-family: "Segoe UI", Roboto, Arial, sans-serif;
    background-color: #f5f7fb;
    color: #1a1a1a;
}

/* Layout */
.container {
    display: flex;
}

/* Sidebar */
.sidebar {
    width: 260px;
    height: 100vh;
    background-color: #0f172a;
    color: white;
    position: fixed;
    padding: 20px;
    overflow-y: auto;
}

.sidebar h2 {
    font-size: 18px;
    margin-bottom: 20px;
}

.sidebar a {
    display: block;
    color: #cbd5f5;
    text-decoration: none;
    margin: 10px 0;
    font-size: 14px;
}

.sidebar a:hover {
    color: white;
}

/* Content */
.content {
    margin-left: 260px;
    padding: 40px;
    width: 100%;
}

h1 {
    color: #0f172a;
}

h2 {
    border-bottom: 2px solid #e2e8f0;
    padding-bottom: 5px;
    margin-top: 40px;
}

.card {
    background: white;
    padding: 20px;
    margin-top: 15px;
    border-radius: 10px;
    box-shadow: 0 4px 10px rgba(0,0,0,0.05);
}

ul {
    line-height: 1.8;
}

.table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 15px;
}

.table th, .table td {
    border: 1px solid #e2e8f0;
    padding: 10px;
    text-align: left;
}

.table th {
    background-color: #f1f5f9;
}

.badge {
    background-color: #2563eb;
    color: white;
    padding: 3px 8px;
    border-radius: 6px;
    font-size: 12px;
}
</style>
</head>

<body>

<div class="container">

<!-- Sidebar -->
<div class="sidebar">
    <h2>AWS IAM Guide</h2>
    <a href="#scope">Scope & Audience</a>
    <a href="#concepts">Key Concepts</a>
    <a href="#implementation">Implementation</a>
    <a href="#iam">Direct IAM</a>
    <a href="#best">Best Practices</a>
    <a href="#glossary">Glossary</a>
</div>

<!-- Content -->
<div class="content">

<h1>AWS IAM & IAM Identity Center</h1>

<!-- Scope -->
<h2 id="scope">1. Scope & Audience</h2>
<div class="card">
<p><strong>Audience:</strong> Cloud, DevOps, and Security Engineers</p>

<p><strong>Scope:</strong></p>
<ul>
<li><span class="badge">Recommended</span> IAM Identity Center (SSO for humans)</li>
<li>IAM (Direct) for workloads and emergency access</li>
</ul>
</div>

<!-- Concepts -->
<h2 id="concepts">2. Key Concepts</h2>
<div class="card">
<ul>
<li><strong>Identity Provider (IdP):</strong> Authentication system (MFA, login)</li>
<li><strong>IAM Identity Center:</strong> Central access management</li>
<li><strong>Permission Set:</strong> Template that creates IAM roles</li>
<li><strong>IAM Role:</strong> Actual identity used inside AWS</li>
<li><strong>IAM (Direct):</strong> Used for workloads, not humans</li>
</ul>
</div>

<!-- Implementation -->
<h2 id="implementation">3. Implementation Steps</h2>

<div class="card">
<h3>Step 1 — AWS Organizations</h3>
<ul>
<li>Create accounts: Dev, Test, Prod</li>
<li>Add Log Archive & Security accounts</li>
</ul>
</div>

<div class="card">
<h3>Step 2 — Enable Identity Center</h3>
<ul>
<li>Enable in selected region</li>
<li>Integrate with organization</li>
</ul>
</div>

<div class="card">
<h3>Step 3 — Identity Source</h3>
<ul>
<li>Built-in directory OR External IdP (SAML/OIDC)</li>
</ul>
</div>

<div class="card">
<h3>Step 4 — Groups</h3>
<ul>
<li>AWS-Admins</li>
<li>AWS-Developers</li>
<li>AWS-ReadOnly</li>
</ul>
</div>

<div class="card">
<h3>Step 5 — Permission Sets</h3>

<table class="table">
<tr><th>Role</th><th>Permissions</th></tr>
<tr><td>Admin</td><td>AdministratorAccess</td></tr>
<tr><td>PowerUser</td><td>PowerUserAccess</td></tr>
<tr><td>ReadOnly</td><td>ReadOnlyAccess</td></tr>
</table>

</div>

<div class="card">
<h3>Step 6 — Assign Access</h3>
<ul>
<li>Map Groups → Permission Sets → Accounts</li>
</ul>
</div>

<div class="card">
<h3>Step 7 — Roles Created</h3>
<p>AWS automatically creates roles like:</p>
<code>AWSReservedSSO_*</code>
</div>

<div class="card">
<h3>Step 8 — Access Flow</h3>
<ul>
<li>Login via portal</li>
<li>Select account</li>
<li>Access console or CLI</li>
</ul>
</div>

<!-- IAM -->
<h2 id="iam">4. Direct IAM</h2>
<div class="card">
<p><strong>Use Cases:</strong></p>
<ul>
<li>EC2 / Lambda / ECS roles</li>
<li>Break-glass access</li>
<li>Legacy systems</li>
</ul>

<p><strong>Avoid:</strong> Human IAM users & long-term keys</p>
</div>

<!-- Best Practices -->
<h2 id="best">5. Best Practices</h2>
<div class="card">
<ul>
<li>Least privilege access</li>
<li>Enable MFA</li>
<li>Use SCPs</li>
<li>Enable CloudTrail</li>
<li>Quarterly access reviews</li>
</ul>
</div>

<!-- Glossary -->
<h2 id="glossary">6. Glossary</h2>
<div class="card">
<ul>
<li><strong>AuthN:</strong> Authentication</li>
<li><strong>AuthZ:</strong> Authorization</li>
<li><strong>SSO:</strong> Single Sign-On</li>
<li><strong>SCP:</strong> Service Control Policy</li>
</ul>
</div>

</div>
</div>

</body>
</html>
