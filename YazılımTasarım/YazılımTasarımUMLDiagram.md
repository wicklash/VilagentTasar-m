classDiagram
    class UserInterface {
        +submitTask()
        +showProgress()
        +showResult()
    }

    class DeepAgent {
        +analyzeTask()
        +retrieveContext()
        +createSubTasks()
        +delegateTask()
        +combineResults()
    }

    class SubAgent {
        +executeSubTask()
        +invokeTool()
        +returnReport()
    }

    class RAGService {
        +retrieveRelevantDocs()
        +retrievePastWorkflows()
        +buildContext()
    }

    class Retriever {
        +search(query)
    }

    class VectorStore {
        +similaritySearch()
        +storeEmbeddings()
    }

    class KnowledgeBase {
        +documents
        +workflowRecords
        +memoryNotes
    }

    class ContextBundle {
        +retrievedDocs
        +workflowHints
        +memorySummary
    }

    class MultiServerMCPClient {
        +session(serverName)
        +get_tools()
        +get_prompt()
        +get_resources()
    }

    class MCPTool {
        +name
        +run(input)
    }

    class MCPServer {
        +exposeTools()
        +processRequest()
    }

    class ToolCallInterceptor {
        +intercept()
    }

    class Callbacks {
        +onProgress()
        +onMessage()
        +onInitialized()
    }

    class ActionExecutor {
        +executeAction()
    }

    class AppController {
        +dispatchCommand()
        +collectState()
    }

    class Receiver {
        +click()
        +typeText()
        +switchWindow()
    }

    class StateManager {
        +updateState()
        +storeResult()
        +sendFeedback()
    }

    UserInterface --> DeepAgent

    DeepAgent --> RAGService
    RAGService --> Retriever
    Retriever --> VectorStore
    Retriever --> KnowledgeBase
    RAGService --> ContextBundle
    ContextBundle --> DeepAgent

    DeepAgent --> SubAgent
    SubAgent --> MultiServerMCPClient : uses MCP tools
    MultiServerMCPClient --> MCPTool : loads
    MultiServerMCPClient --> MCPServer : connects
    MultiServerMCPClient --> ToolCallInterceptor
    MultiServerMCPClient --> Callbacks

    MCPTool --> MCPServer : backed by
    MCPServer --> ActionExecutor
    ActionExecutor --> AppController
    AppController --> Receiver

    Receiver --> StateManager
    StateManager --> DeepAgent