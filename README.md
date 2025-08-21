
#注意 此项目我仅仅进行了美化，用于自用，fork的one point！！！！！！！ 

### **开源项目：Two-Point/One-Point - Cloudflare Workers 上的多功能文件索引器**

Two-Point（或稱 One-Point）是一个部署在 Cloudflare Workers 上的文件索引程序，它可以帮助您轻松地索引和管理存储在各种云存储服务或本地文件系统中的文件，并通过一个简洁的网页界面进行访问。

#### **简介**

该项目利用 Cloudflare 的全球网络，为您提供快速、可靠的文件访问体验。它支持多种主流云存储服务，并提供主题定制、密码保护和在线预览等功能。通过简单的配置，您可以将分散在各处的文件资源整合到一个统一的界面中进行管理。

#### **功能特性**

*   **多驱动支持**：支持包括 Google Drive、OneDrive、阿里云盘、Teambition、Coding 等在内的多种云存储服务，以及本地文件系统（`node_fs`）。
*   **无服务器架构**：部署在 Cloudflare Workers 上，无需管理自己的服务器。
*   **在线文件预览**：支持图片、视频、音频和文本文档的在线预览。
*   **密码保护**：可以为特定的云盘驱动器或目录设置密码保护。
*   **主题定制**：支持自定义主题，美化您的文件索引页面。
*   **管理后台**：提供一个管理后台，方便您在线配置站点信息和云盘驱动器。
*   **缓存机制**：利用缓存来提高文件列表和文件信息的加载速度。

---

### **使用教程**

#### **第一步：准备工作**

1.  **Cloudflare 账号**：您需要一个 Cloudflare 账号。
2.  **Wrangler CLI**：确保您已经安装了 Cloudflare 的命令行工具 Wrangler。如果没有，可以通过 npm 进行安装：`npm install -g wrangler`。
3.  **项目代码**：从项目的 GitHub 仓库获取 `workers.js` 文件。

#### **第二步：创建 Cloudflare Worker 和 KV Namespace**

1.  **登录 Wrangler**：在您的终端中运行 `wrangler login`，并按照提示完成登录。

2.  **创建 KV Namespace**：您需要一个 KV Namespace 来存储配置文件。运行以下命令创建一个名为 `OPCONFIG` 的 KV Namespace：
    ```bash
    wrangler kv:namespace create "OPCONFIG"
    ```
    记下命令输出中的 `id`，您将在下一步中使用它。

3.  **创建 `wrangler.toml` 配置文件**：在您的项目目录中创建一个名为 `wrangler.toml` 的文件，并填入以下内容：
    ```toml
    name = "your-worker-name" # 替换为您想要的 Worker 名称
    main = "workers.js"
    compatibility_date = "2023-01-01" # 使用一个近期的日期

    kv_namespaces = [
      { binding = "OPCONFIG", id = "your_kv_namespace_id" } # 将 id 替换为您上一步中获取的 id
    ]
    ```

#### **第三步：部署 Worker**

1.  将从 GitHub 获取的 `workers.js` 文件放置在与 `wrangler.toml` 相同的目录中。

2.  运行以下命令来部署您的 Worker：
    ```bash
    wrangler publish
    ```

#### **第四步：配置**

1.  **首次配置**：部署成功后，访问您的 Worker URL（例如 `https://your-worker-name.your-workers-subdomain.workers.dev/admin/`）进入管理后台。首次访问时，系统会引导您进行初始化配置，包括设置管理员用户名和密码，以及一个用于签名的 `salt`。

2.  **添加云盘驱动器**：
    *   在管理后台的“Drives”部分，您可以添加和管理不同的云盘驱动器。
    *   点击“Add Drive”按钮。
    *   **Mount Path**：为您要挂载的云盘设置一个路径，例如 `/googledrive`。
    *   **Module**：选择您要使用的云存储模块（例如 `googledrive`、`onedrive` 等）。
    *   根据所选模块的要求，填写相应的配置信息，例如 `refresh_token`、`root` 目录 ID 等。
    *   **Password**（可选）：为该云盘设置访问密码。
    *   **Readme**（可选）：为该云盘设置一个 Markdown 格式的说明。

3.  **站点配置**：
    *   在“Basic”配置页面，您可以自定义站点名称、Logo、主题以及首页的 README 内容。
    *   **Theme**：选择一个喜欢的主题来改变文件索引页面的外观。

#### **第五步：使用**

1.  **浏览文件**：配置完成后，访问您的 Worker URL，您将看到已配置的文件索引。您可以像浏览本地文件系统一样浏览不同云盘中的文件和文件夹。

2.  **文件操作**：根据不同模块的支持情况，您可以进行文件的下载、预览等操作。

3.  **分享链接**：您可以直接分享文件的 URL 链接给他人。如果设置了密码保护，访问者需要输入正确的密码才能访问。
