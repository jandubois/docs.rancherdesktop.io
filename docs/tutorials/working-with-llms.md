---
title: Working with LLMs using Open WebUI
---

The **Open WebUI extension** for Rancher Desktop provides an easy-to-install package for local AI and Large Language Model (LLM) development. It includes the following components:

-   **[Ollama](https://ollama.com/):** For pulling, running, and fine-tuning open-source LLMs.
-   **[Open WebUI](https://openwebui.com/):** A feature-rich user interface for interacting with LLMs.
-   **[tinyllama](https://ollama.com/library/tinyllama):** A lightweight LLM to help you get started quickly.
-   **[SearXNG](https://github.com/searxng/searxng):** A metasearch engine to enhance Retrieval-Augmented Generation (RAG) with live web search results.
-   **[mcpo](https://github.com/open-webui/mcpo):** For running Model Context Protocol (MCP) servers in a container.

## Installing the Open WebUI Extension

### Prerequisites

-   **Rancher Desktop:** Version 1.17 or newer.
-   **Disk Space:** At least 7 GB available.
-   **Memory:** 16 GB RAM is recommended for optimal performance.
-   **Ports:** The extension requires host ports `11434`, `11500`, and `11505` to be available for the Ollama, Open WebUI, and SearXNG services, respectively.

### Installation Steps

You can install the Open WebUI extension from the Extensions Catalog in the Rancher Desktop UI or by using the `rdctl` command-line tool.

> **Note:** The installation process may take several minutes, depending on your internet connection and machine performance.

<Tabs groupId="container-runtime">
<TabItem value="Extensions Catalog (GUI)" default>

1.  In the Rancher Desktop UI, navigate to the **Extensions** page.
2.  From the **Catalog** tab, find the **Open WebUI** extension and click **Install**.

    ![](../img/working-with-open-webui/install-extension.png)

</TabItem>
<TabItem value="CLI">

Run the following command to install the extension:

```bash
rdctl extension install ghcr.io/rancher-sandbox/rancher-desktop-rdx-open-webui:latest
```

> **Caution:** We recommend using a specific release tag instead of `latest` to ensure compatibility with your version of Rancher Desktop. You can find available release tags on the [GitHub Tags page](https://github.com/rancher-sandbox/rancher-desktop-rdx-open-webui/tags).

</TabItem>
</Tabs>

> **A Note on Ollama:**
>
> -   If Ollama is already installed on your machine in the default location, the extension will use the existing installation. Otherwise, it will be installed into the extension's directory.
> -   Ollama will automatically use the GPU on machines with an NVIDIA GPU (and the necessary drivers) or an Apple Silicon CPU.

## Using Open WebUI

This section provides an overview of the most commonly used features in Open WebUI. For more detailed information, please refer to the [official Open WebUI documentation](https://docs.openwebui.com).

### Managing Models

#### Pulling Models from the Ollama Library

The Open WebUI extension comes with `tinyllama`, a lightweight LLM that is perfect for getting started. For more advanced tasks, you will want to download more powerful models, such as Llama, Mistral, or Gemma.

You can pull models from the [Ollama Models Library](https://ollama.com/library) directly from the Open WebUI dashboard:

1.  Navigate to **Admin Panel > Settings > Connections**.
2.  In the "Pull a model from Ollama.com" field, enter the name and tag of the model you want to download (e.g., `llama3:latest`).
3.  Click the **Pull** button.

![](../img/working-with-open-webui/pulling-models.png)

#### Using GGUF Models from Hugging Face

GGUF (GPT-Generated Unified Format) is a binary format that is optimized for fast loading and efficient inference. You can easily use GGUF models from Hugging Face with Ollama.

1.  Find a model in GGUF format on Hugging Face and navigate to its page.
2.  Click the **Use this model** button and select **Ollama**.
3.  From the dialog that appears, copy the full model name and tag (e.g., `hf.co/QuantFactory/Ministral-3b-instruct-GGUF:Q4_K_M`).

    > **Tip:** The `Q4_K_M` quantization of GGUF models generally provides a good balance between size and performance for local use.

    ![](../img/working-with-open-webui/gguf-model-page-hf.png)

4.  In the Open WebUI dashboard, navigate to **Admin Panel > Settings > Connections**.
5.  Paste the full model name into the "Pull a model" field and click the **Pull** button.

    ![](../img/working-with-open-webui/download-gguf-from-hf.png)

#### Using OpenAI-Compatible APIs

You can also connect to OpenAI-compatible APIs from local servers (e.g., [LocalAI](https://localai.io/), [llamafile](https://github.com/Mozilla-Ocho/llamafile)) or cloud providers (e.g., [Groq](https://groq.com/), [OpenAI](https://openai.com/api/)).

For example, to connect to the Groq API:

1.  Create a Groq Cloud account at https://console.groq.com.
2.  Generate an API key from the API Keys page.
3.  In the Open WebUI dashboard, navigate to **Admin Panel > Settings > Connections**.
4.  Add Groq as an OpenAI-compatible API provider, using `https://api.groq.com/openai/v1` as the base URL and the API key you generated.

You will now be able to select and use models from Groq in the model selector dropdown.

![](../img/working-with-open-webui/openai-api-setting.png)
![](../img/working-with-open-webui/groq-models-list.png)

### Interacting with the local LLMs

### Interacting with LLMs

Open WebUI provides two main interfaces for interacting with LLMs: the Chat interface and the Playground.

#### Chat Interface

The Chat interface provides a familiar, ChatGPT-style experience for interacting with your local LLMs. You can select the model you want to use from the dropdown menu. You can also select multiple models to interact with them simultaneously by clicking the `+` button next to the model selector.

![](../img/working-with-open-webui/multi-model-chat.png)

A unique feature of the Open WebUI chat interface is its ability to render a live preview of HTML, CSS, and JavaScript code snippets included in a chat response.

![](../img/working-with-open-webui/live-html-preview.png)

#### Playground Interface

The Playground provides a more advanced interface for interacting with LLMs, with separate features for Chat and Text Completion. In the Playground, you can provide a system prompt to guide the LLM's responses or use the text completion feature to have the LLM complete an initial prompt.

![](../img/working-with-open-webui/playground-chat.png)
![](../img/working-with-open-webui/playground-sentence-completion.png)

### Customizing Model Behavior

You can customize the behavior of your models by adjusting LLM parameters and providing a custom system prompt. These settings can be applied on a per-chat, per-model, or per-account basis. For a complete list of available parameters, please refer to the [Ollama Model File documentation](https://github.com/ollama/ollama/blob/main/docs/modelfile.md#parameter).

![](../img/working-with-open-webui/setting-chat-parameters.png)

### Retrieval-Augmented Generation (RAG)

Open WebUI supports Retrieval-Augmented Generation (RAG), which allows you to augment LLM responses with custom knowledge from local documents or live web search results.

#### Knowledge Collections

You can create knowledge collections by uploading your own documents. To manage your knowledge collections, navigate to **Workspace > Knowledge**.

![](../img/working-with-open-webui/knowedge-collections.png)

To use a knowledge collection in a chat, type `#` in the chat input field and select one or more collections from the list. You can also include the content of a web page by typing `#` followed by the URL.

![](../img/working-with-open-webui/selecting-knowedge-collections.png)

#### Web Search

The Open WebUI extension includes SearXNG, a metasearch engine that allows you to augment LLM responses with live web search results. To use web search in a chat, click the `+` button to the left of the chat input field and enable the **Web Search** option.

![](../img/working-with-open-webui/enabling-web-search.png)

> **Tip:** You can use both knowledge collections and web search to augment the LLM's response to a single prompt.

### Customizing Models

You can create custom models tailored to your specific needs by navigating to the **Workspace > Models** page. Here, you can extend existing models, tweak their parameters, and include knowledge collections. These custom models can then be used in the Chat and Playground interfaces.

![](../img/working-with-open-webui/customize-models.png)

### Using the Model Context Protocol (MCP)

The [Model Context Protocol (MCP)](https://modelcontextprotocol.io/introduction) is an open standard for connecting LLMs to external tools and data sources. The Open WebUI extension includes `mcpo`, an MCP-to-OpenAPI proxy, and two sample MCP servers for interacting with Docker and Kubernetes.

> **Note:** To use the Docker and Kubernetes MCP servers, you must have the `dockerd (moby)` runtime and Kubernetes enabled, respectively.

#### Enabling the Bundled MCP Servers

1.  Navigate to **Admin Panel > Settings > Tools**.
2.  Click the `+` button to add a new MCP server tool.
3.  Add the Docker MCP server with the URL `http://host.docker.internal:11600/docker-mcp`.
4.  Add the Kubernetes MCP server with the URL `http://host.docker.internal:11600/kubernetes`.
5.  Navigate to **Admin Panel > Settings > Models** and click the pencil icon to edit the model you want to use.
6.  Under **Advanced Params**, set **Function Calling** to `Native`.
7.  Under **Tools**, select the **Docker-MCP** and **Kubernetes-Mcp-Server** tools.
8.  Click **Save & Update**.
9.  Start a new chat with the model you just edited and try a prompt such as, `List my docker volumes in a table`.

#### Adding New MCP Servers

You can add other MCP servers by editing the `config.json` file in the `mcpo` container.

> **Important:** Do not install MCP servers from untrusted sources.

1.  Find the `config.json` file in `<extension-installation-dir>/compose/linux/mcpo/config.json`.
2.  Add the new server configuration to the `mcpServers` object.
3.  Restart the `mcpo` container from the **Containers** page in the Rancher Desktop UI.
4.  Follow the steps in the previous section to add the new MCP server as a tool in the Open WebUI dashboard. The URL for the new tool will be `http://host.docker.internal:11600/<mcp-server-name>`.

## Uninstalling the Extension

You can uninstall the Open WebUI extension from the Extensions Catalog in the Rancher Desktop UI or by using the `rdctl` command-line tool.

> **Note:**
>
> -   If Ollama was installed by the extension, it will be removed during the uninstallation process.
> -   Ollama model files are not removed automatically. If you wish to delete them, you will need to manually delete the `~/.ollama/models` directory (on macOS and Linux) or the `%USERPROFILE%/.ollama/models` directory (on Windows).

<Tabs groupId="container-runtime">
<TabItem value="Extensions Catalog (GUI)" default>

1.  In the Rancher Desktop UI, navigate to the **Extensions** page.
2.  From either the **Catalog** or **Installed** tab, find the **Open WebUI** extension and click **Uninstall**.

    ![](../img/working-with-open-webui/uninstall-extension-from-catalog-tab.png)
    ![](../img/working-with-open-webui/uninstall-extension-from-installed-tab.png)

</TabItem>
<TabItem value="CLI">

Run the following command to uninstall the extension:

```bash
rdctl extension uninstall ghcr.io/rancher-sandbox/rancher-desktop-rdx-open-webui:<tag>
```

</TabItem>
</Tabs>
