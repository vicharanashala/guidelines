# Programming Style Guidelines
**For Application Development and AI Engineering**

**Annam & Vicharanashala**

Version 1.0  
February 2026

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [General Rules (Common to All Teams)](#2-general-rules-common-to-all-teams)
3. [Naming Conventions](#3-naming-conventions)
4. [Part I: Application Development Guidelines](#part-i-application-development-guidelines)
5. [Part II: AI Engineering Guidelines](#part-ii-ai-engineering-guidelines)
6. [File Organization](#6-file-organization)
7. [Code Formatting and Layout](#7-code-formatting-and-layout)
8. [Testing Standards](#8-testing-standards)
9. [API Design Standards](#9-api-design-standards)
10. [Version Control and Collaboration](#10-version-control-and-collaboration)
11. [Security and Configuration](#11-security-and-configuration)
12. [Documentation](#12-documentation)

---

## 1. Introduction

### 1.1 Purpose

This document defines the programming style and development guidelines for all software projects developed under Annam and Vicharanashala. These guidelines apply to:

- **Application Development teams** (MERN stack with TypeScript)
- **AI Engineering teams** (Python, FastAPI, ML/LLM systems)

The purpose of this guideline is to:

- Improve readability and consistency of code
- Increase maintainability and reliability
- Standardize tooling and workflows
- Reduce friction during code reviews and onboarding
- Establish clear expectations for all developers

### 1.2 Scope

These guidelines apply to:

- All new projects (MUST be followed from inception)
- All existing projects (progressive migration is expected)
- All developers working on projects from Annam and Vicharanashala

### 1.3 Recommendation Importance

The following keywords define the importance of each recommendation:

- **MUST** – Mandatory requirement. Non-compliance will result in PR rejection.
- **SHOULD** – Strong recommendation. Exceptions allowed with justification.
- **CAN** – General guideline. Use judgment based on context.
- **NOT ALLOWED** – Explicitly forbidden.

Any rule MAY be violated if doing so clearly improves readability or correctness, but such violations MUST be justified in the PR description.

### 1.4 Enforcement

- Non-compliance with MUST requirements will result in PR rejection or change requests
- Repeated non-compliance may result in additional code review requirements
- Review feedback SHOULD be treated as an opportunity to improve code quality
- Merge conflicts SHOULD be resolved together with a maintainer

---

## 2. General Rules (Common to All Teams)

### 2.1 Technology Stack

#### Application Development
1. Application development MUST use **TypeScript only**. JavaScript is NOT ALLOWED for production code.

2. Backend API development MUST use:
   - **NestJS** for all new services
   - **Express** (via `routing-controllers` library) ONLY for existing legacy services

3. Frontend development MUST use:
   - **React** (function components only)
   - **TanStack Query** (mandatory for server state)
   - **TanStack Router** and related TanStack libraries (mandatory)

4. **Bun.js** and **Deno** are NOT ALLOWED.

**Motivation:**  
TypeScript provides type safety and better developer experience. NestJS provides structure and testability. TanStack libraries are industry-standard for React data fetching and routing.

#### AI Engineering
5. Python API development MUST use **FastAPI**.

6. **Pydantic** MUST be used for request and response validation in Python APIs.

7. **PyTorch** MUST be used only where the project explicitly requires it.

8. LLM-based systems MUST use:
   - **LangChain** or **LangGraph** in Python/Typescript.

9. Standard **PEP8** conventions MUST be followed for all Python code.

**Motivation:**  
FastAPI provides automatic OpenAPI documentation and high performance. Pydantic ensures data validation. LangChain/LangGraph are industry standards for LLM applications.

### 2.2 Package Management

#### TypeScript/Node.js
10. **npm** or **pnpm** SHOULD be used for package management.

11. `package-lock.json` or `pnpm-lock.yaml` MUST be committed to version control.

**Motivation:**  
Lock files ensure reproducible builds across environments.

#### Python
12. **uv** MUST be the default package manager for all new Python projects.

13. Existing projects MUST progressively transition to **uv**.

14. `requirements.txt` CAN be used for deployment purposes only.

**Motivation:**  
`uv` is significantly faster and more reliable than pip. Standardizing on one tool reduces tooling complexity.

### 2.3 Tooling Requirements

The following tools are mandatory for all projects:

#### TypeScript Projects
15. **ESLint** MUST be configured and enforced.
   ```json
   // Minimum configuration
   {
     "extends": ["eslint:recommended", "plugin:@typescript-eslint/recommended"],
     "parser": "@typescript-eslint/parser"
   }
   ```

16. **Prettier** MUST be configured for consistent formatting.
   ```json
   // .prettierrc
   {
     "semi": true,
     "singleQuote": false,
     "tabWidth": 2,
     "printWidth": 100
   }
   ```

17. **Vitest** MUST be used for all testing (frontend and backend).

**Motivation:**  
ESLint catches bugs before runtime. Prettier eliminates style debates. Vitest provides fast, modern testing with TypeScript support.

#### Python Projects
18. **Black** MUST be used for code formatting.
   ```toml
   # pyproject.toml
   [tool.black]
   line-length = 100
   ```

19. **mypy** SHOULD be used for type checking in core modules.
   ```ini
   # myproject.ini
   [mypy]
   python_version = 3.11
   warn_return_any = True
   warn_unused_configs = True
   disallow_untyped_defs = True
   ```

**Motivation:**  
Black removes formatting discussions. mypy catches type errors before runtime.

#### All Projects
20. **Husky** MUST be configured for pre-commit hooks.
   ```json
   // package.json
   {
     "husky": {
       "hooks": {
         "pre-commit": "lint-staged",
         "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
       }
     }
   }
   ```

**Motivation:**  
Pre-commit hooks prevent broken code from being committed, reducing CI failures.

### 2.4 Testing Requirements

21. All backend services MUST have **integration tests**.

22. All tests MUST use **Vitest**.

23. **Minimum test coverage MUST be 90%**.

24. Tests MUST pass before a PR can be merged.

25. A pull request that breaks existing tests MUST be rejected or returned for changes.

**Motivation:**  
High test coverage prevents regressions. Integration tests validate that components work together correctly. Vitest provides fast execution and modern features.

### 2.5 Documentation Requirements

26. All backend APIs MUST adhere to the **OpenAPI 3.0** standard.

27. API documentation MUST exist at the **controller level**.

28. All public functions/methods SHOULD have documentation comments.

29. Complex algorithms or business logic MUST be documented with comments explaining the "why", not the "what".

**Motivation:**  
OpenAPI ensures machine-readable API specs, enabling automatic client generation and testing. Controller-level docs keep documentation close to implementation.

### 2.6 Use of AI Tools

30. Use of AI-assisted tools (GitHub Copilot, ChatGPT, Claude, etc.) is **allowed and NOT restricted**.

31. Code generated using AI tools MUST be reviewed and refactored to comply with these guidelines.

32. Submitting non-compliant AI-generated code will result in PR rejection with change requests.

33. Developers SHOULD understand all code they submit, regardless of how it was generated.

**Motivation:**  
AI tools boost productivity, but generated code must meet organizational standards. Developers remain responsible for all submitted code.

---

## 3. Naming Conventions

### 3.1 General Naming Conventions

#### Variables
34. Variable names MUST use **camelCase** in TypeScript.
   ```typescript
   // CORRECT
   const totalCount = 0;
   const userProfile = getUserProfile();
   
   // INCORRECT
   const total_count = 0;
   const TotalCount = 0;
   ```

35. Variable names MUST use **snake_case** in Python.
   ```python
   # CORRECT
   total_count = 0
   user_profile = get_user_profile()
   
   # INCORRECT
   totalCount = 0
   TotalCount = 0
   ```

**Motivation:**  
Following language conventions makes code readable to developers familiar with the ecosystem.

#### Classes
36. Class names MUST use **PascalCase** in both TypeScript and Python.
   ```typescript
   class UserService { }
   class OrderController { }
   ```
   
   ```python
   class UserService:
       pass
   
   class PredictionPipeline:
       pass
   ```

**Motivation:**  
PascalCase for classes is universal across programming languages.

#### Constants
37. Named constants MUST use **UPPER_SNAKE_CASE** in both languages.
   ```typescript
   const MAX_RETRIES = 3;
   const DEFAULT_TIMEOUT = 5000;
   const API_BASE_URL = "https://api.example.com";
   ```
   
   ```python
   MAX_RETRIES = 3
   DEFAULT_TIMEOUT = 5000
   API_BASE_URL = "https://api.example.com"
   ```

38. Magic numbers MUST be avoided in code. Use named constants instead.
   ```typescript
   // INCORRECT
   if (retries > 3) { }
   
   // CORRECT
   const MAX_RETRIES = 3;
   if (retries > MAX_RETRIES) { }
   ```

**Motivation:**  
Named constants make code self-documenting and easier to maintain.

#### Boolean Variables
39. Boolean variable names MUST start with **is**, **has**, **can**, or **should**.
   ```typescript
   // CORRECT
   const isValid = true;
   const hasPermission = checkPermission();
   const canEdit = user.role === 'admin';
   const shouldRefresh = cacheExpired();
   
   // INCORRECT
   const valid = true;
   const permission = checkPermission();
   ```

**Motivation:**  
Prefix makes boolean intent immediately clear and reads naturally in conditionals.

#### File Names
40. File names MUST use **camelCase** or **PascalCase** in TypeScript.
   ```
   userService.ts       (preferred for non-class files)
   UserService.ts       (for files containing a single class)
   authController.ts
   AuthController.ts
   ```

41. File names MUST use **snake_case** in Python.
   ```
   user_service.py
   auth_controller.py
   prediction_pipeline.py
   ```

**Motivation:**  
Follows language ecosystem conventions.

### 3.2 TypeScript-Specific Naming

#### React Components
42. React component names MUST use **PascalCase**.
   ```typescript
   // Component file: LoginForm.tsx
   const LoginForm: React.FC = () => { };
   
   // Component file: DashboardView.tsx
   const DashboardView: React.FC = () => { };
   ```

**Motivation:**  
React requires components to start with uppercase. PascalCase is the universal React convention.

#### Hooks
43. React hooks MUST be prefixed with **use**.
   ```typescript
   // CORRECT
   const useAuth = () => { };
   const useUserProfile = () => { };
   
   // INCORRECT
   const auth = () => { };
   const getUserProfile = () => { };
   ```

**Motivation:**  
The `use` prefix is required by React's Rules of Hooks.

#### State Management
44. Zustand stores SHOULD use semantic names with the **use** prefix.
   ```typescript
   // CORRECT
   const useAuthStore = create((set) => ({ }));
   const useCartStore = create((set) => ({ }));
   ```

**Motivation:**  
Consistent naming makes stores discoverable and maintains hook conventions.

#### Services and Controllers
45. Service classes MUST be suffixed with **Service**.
   ```typescript
   class UserService { }
   class PaymentService { }
   ```

46. Controller classes MUST be suffixed with **Controller**.
   ```typescript
   @Controller('/users')
   class UserController { }
   
   @Controller('/orders')
   class OrderController { }
   ```

**Motivation:**  
Suffixes make architectural layers immediately recognizable.

### 3.3 Python-Specific Naming

#### Functions and Methods
47. Function and method names MUST use **snake_case**.
   ```python
   def get_user_profile(user_id: int) -> UserProfile:
       pass
   
   def calculate_total_price(items: list[Item]) -> float:
       pass
   ```

**Motivation:**  
Follows PEP8 conventions.

#### Private Members
48. Private attributes and methods SHOULD be prefixed with a **single underscore**.
   ```python
   class UserService:
       def __init__(self):
           self._cache = {}  # Private attribute
       
       def _validate_user(self, user: User) -> bool:  # Private method
           pass
   ```

**Motivation:**  
Indicates internal implementation details that should not be accessed externally.

### 3.4 Abbreviations and Acronyms

49. Abbreviations and acronyms MUST NOT be uppercase when used in names.
   ```typescript
   // CORRECT
   exportHtmlSource();
   openDvdPlayer();
   const userId = 123;
   
   // INCORRECT
   exportHTMLSource();
   openDVDPlayer();
   const userID = 123;
   ```

**Motivation:**  
Preserves readability when names are concatenated.

### 3.5 Collections and Plurals

50. Collection variable names SHOULD use plural form.
   ```typescript
   // CORRECT
   const users = await getUsers();
   const orderIds = orders.map(o => o.id);
   
   // INCORRECT
   const userList = await getUsers();
   const orderIdArray = orders.map(o => o.id);
   ```

**Motivation:**  
Plural form is more natural and concise.

### 3.6 Iterators and Counters

51. Loop iterators SHOULD use conventional names: `i`, `j`, `k` for simple loops.
   ```typescript
   for (let i = 0; i < items.length; i++) {
       for (let j = 0; j < items[i].length; j++) {
           // ...
       }
   }
   ```

52. Semantic iterator names SHOULD be used for complex iterations.
   ```typescript
   // CORRECT
   for (const user of users) {
       console.log(user.name);
   }
   
   users.forEach((user) => {
       console.log(user.name);
   });
   ```

**Motivation:**  
Simple iterators don't need descriptive names. Complex iterations benefit from semantic names.

---

## Part I: Application Development Guidelines

### 4. Application Development Stack

#### 4.1 Backend Framework

53. New backend services MUST use **NestJS**.

54. Existing services using **Express** with `routing-controllers` MAY continue, but SHOULD migrate to NestJS over time.

55. Controllers MUST be thin and contain primarily:
    - Authorization logic
    - Request validation
    - Delegation to services

56. Business logic MUST reside in **Service** classes.

**Example:**
```typescript
// CORRECT - Thin controller
@Controller('/users')
export class UserController {
    constructor(
        private readonly userService: UserService,
        private readonly caslAbilityFactory: CaslAbilityFactory
    ) {}

    @Get(':id')
    async getUser(
        @Param('id') id: string,
        @CurrentUser() user: User
    ): Promise<UserDto> {
        // Authorization
        const ability = this.caslAbilityFactory.createForUser(user);
        if (!ability.can('read', 'User')) {
            throw new ForbiddenException();
        }

        // Delegate to service
        return this.userService.getUserById(id);
    }
}

// INCORRECT - Fat controller with business logic
@Controller('/users')
export class UserController {
    @Get(':id')
    async getUser(@Param('id') id: string): Promise<UserDto> {
        // ❌ Business logic in controller
        const user = await this.db.users.findOne({ id });
        if (!user) throw new NotFoundException();
        
        const profile = await this.db.profiles.findOne({ userId: id });
        const orders = await this.db.orders.find({ userId: id });
        
        return {
            ...user,
            profile,
            orderCount: orders.length
        };
    }
}
```

**Motivation:**  
Thin controllers improve testability, reusability, and separation of concerns.

#### 4.2 Authorization

57. Authorization MUST use **CASL** (Code Access Security Library).

58. Permission checks MUST happen at the controller level before delegating to services.

**Example:**
```typescript
import { AbilityBuilder, Ability } from '@casl/ability';

export class CaslAbilityFactory {
    createForUser(user: User) {
        const { can, cannot, build } = new AbilityBuilder(Ability);

        if (user.role === 'admin') {
            can('manage', 'all');
        } else {
            can('read', 'User', { id: user.id });
            can('update', 'User', { id: user.id });
        }

        return build();
    }
}
```

**Motivation:**  
CASL provides type-safe, testable authorization logic.

#### 4.3 Error Handling

59. Error handling MUST be centralized using middleware.

60. Custom error classes SHOULD extend base error types.

**Example:**
```typescript
// error.filter.ts
@Catch()
export class GlobalExceptionFilter implements ExceptionFilter {
    catch(exception: unknown, host: ArgumentsHost) {
        const ctx = host.switchToHttp();
        const response = ctx.getResponse();
        const request = ctx.getRequest();

        let status = HttpStatus.INTERNAL_SERVER_ERROR;
        let message = 'Internal server error';

        if (exception instanceof HttpException) {
            status = exception.getStatus();
            message = exception.message;
        }

        response.status(status).json({
            statusCode: status,
            message,
            timestamp: new Date().toISOString(),
            path: request.url,
        });
    }
}

// Custom error
export class BusinessLogicError extends Error {
    constructor(message: string) {
        super(message);
        this.name = 'BusinessLogicError';
    }
}
```

**Motivation:**  
Centralized error handling ensures consistent error responses and logging.

### 5. Frontend Guidelines

#### 5.1 Component Structure

61. React components MUST be **function components**.

62. **Class components** are NOT ALLOWED.

63. Components MUST use **Hooks** for state and side effects.

**Example:**
```typescript
// CORRECT
const UserProfile: React.FC<{ userId: string }> = ({ userId }) => {
    const { data, isLoading, error } = useQuery({
        queryKey: ['user', userId],
        queryFn: () => fetchUser(userId),
    });

    if (isLoading) return <Spinner />;
    if (error) return <ErrorMessage error={error} />;
    
    return <div>{data?.name}</div>;
};

// INCORRECT - Class component
class UserProfile extends React.Component<{ userId: string }> {
    // ❌ Class components not allowed
}
```

**Motivation:**  
Function components with hooks are simpler, more composable, and the modern React standard.

#### 5.2 State Management

64. **Server state** MUST be managed using **TanStack Query**.

65. **Local UI state** SHOULD be managed using:
    - React's `useState`/`useReducer` for component-local state
    - **Zustand** for shared UI state across components

66. **Global state** SHOULD be minimized. Prefer lifting state up or using composition.

**Example:**
```typescript
// Server state with TanStack Query
const UserList: React.FC = () => {
    const { data: users } = useQuery({
        queryKey: ['users'],
        queryFn: fetchUsers,
    });

    return <div>{users?.map(u => <UserCard key={u.id} user={u} />)}</div>;
};

// Local UI state
const SearchBar: React.FC = () => {
    const [query, setQuery] = useState('');
    
    return (
        <input 
            value={query} 
            onChange={(e) => setQuery(e.target.value)} 
        />
    );
};

// Shared UI state with Zustand
const useUIStore = create<UIStore>((set) => ({
    sidebarOpen: false,
    toggleSidebar: () => set((state) => ({ sidebarOpen: !state.sidebarOpen })),
}));
```

**Motivation:**  
TanStack Query handles caching, revalidation, and loading states automatically. Zustand provides simple global state without boilerplate.

#### 5.3 Routing

67. Routing MUST use **TanStack Router**.

68. Routes SHOULD be type-safe using TanStack Router's type system.

**Example:**
```typescript
import { createRoute } from '@tanstack/react-router';

const userRoute = createRoute({
    getParentRoute: () => rootRoute,
    path: '/users/$userId',
    component: UserProfile,
});
```

**Motivation:**  
TanStack Router provides type-safe routing with automatic code splitting.

### 6. TypeScript-Specific Rules

#### 6.1 Type Safety

69. **Explicit types** MUST be used for function parameters and return values.
    ```typescript
    // CORRECT
    function calculateTotal(items: Item[]): number {
        return items.reduce((sum, item) => sum + item.price, 0);
    }

    // INCORRECT
    function calculateTotal(items) {
        return items.reduce((sum, item) => sum + item.price, 0);
    }
    ```

70. The **`any` type** SHOULD be avoided. Use `unknown` if type is truly unknown.
    ```typescript
    // INCORRECT
    function process(data: any) { }

    // CORRECT
    function process(data: unknown) {
        if (typeof data === 'string') {
            // Type narrowing
        }
    }
    ```

71. **Type assertions** (`as`) SHOULD be avoided unless necessary. Use type guards instead.
    ```typescript
    // INCORRECT
    const user = data as User;

    // CORRECT
    function isUser(data: unknown): data is User {
        return typeof data === 'object' && data !== null && 'id' in data;
    }
    ```

**Motivation:**  
Type safety prevents runtime errors and improves code maintainability.

#### 6.2 Interfaces vs Types

72. **Interfaces** SHOULD be used for object shapes that may be extended.

73. **Type aliases** SHOULD be used for unions, intersections, and primitives.

**Example:**
```typescript
// Use interface for extensible object shapes
interface User {
    id: string;
    name: string;
}

interface AdminUser extends User {
    permissions: string[];
}

// Use type for unions and complex types
type Status = 'pending' | 'approved' | 'rejected';
type Result<T> = { success: true; data: T } | { success: false; error: string };
```

**Motivation:**  
Interfaces can be extended and merged; types are better for unions and complex compositions.

#### 6.3 Async/Await

74. **Async/await** MUST be used instead of raw promises with `.then()`.

75. Error handling for async operations MUST use **try/catch** blocks.

**Example:**
```typescript
// CORRECT
async function fetchUser(id: string): Promise<User> {
    try {
        const response = await fetch(`/api/users/${id}`);
        if (!response.ok) {
            throw new Error(`HTTP error: ${response.status}`);
        }
        return await response.json();
    } catch (error) {
        console.error('Failed to fetch user:', error);
        throw error;
    }
}

// INCORRECT
function fetchUser(id: string): Promise<User> {
    return fetch(`/api/users/${id}`)
        .then(response => response.json())
        .catch(error => {
            console.error(error);
            throw error;
        });
}
```

**Motivation:**  
Async/await is more readable and easier to debug than promise chains.

#### 6.4 Null and Undefined

76. **Null checks** MUST use strict equality (`===` / `!==`).

77. Optional chaining (`?.`) SHOULD be used when accessing nested properties.

78. Nullish coalescing (`??`) SHOULD be used for default values.

**Example:**
```typescript
// Optional chaining
const userName = user?.profile?.name;

// Nullish coalescing
const displayName = userName ?? 'Anonymous';

// INCORRECT
const displayName = userName || 'Anonymous'; // ❌ Empty string would also use default
```

**Motivation:**  
Optional chaining prevents runtime errors. Nullish coalescing only replaces `null`/`undefined`, not falsy values.

---

## Part II: AI Engineering Guidelines

### 7. Python Development Standards

#### 7.1 FastAPI Structure

79. All Python APIs MUST use **FastAPI**.

80. API routers SHOULD be organized by domain/feature.

**Example:**
```python
# main.py
from fastapi import FastAPI
from app.routers import users, predictions

app = FastAPI()
app.include_router(users.router)
app.include_router(predictions.router)

# app/routers/users.py
from fastapi import APIRouter, Depends

router = APIRouter(prefix="/users", tags=["users"])

@router.get("/{user_id}")
async def get_user(user_id: str) -> UserResponse:
    return await user_service.get_user(user_id)
```

**Motivation:**  
Modular routers improve code organization and maintainability.

#### 7.2 Pydantic Models

81. **Pydantic** models MUST be used for all request and response validation.

82. Validation logic SHOULD be included in Pydantic model validators.

**Example:**
```python
from pydantic import BaseModel, Field, validator

class CreateUserRequest(BaseModel):
    email: str = Field(..., description="User email address")
    age: int = Field(..., ge=18, le=120)
    
    @validator('email')
    def validate_email(cls, v):
        if '@' not in v:
            raise ValueError('Invalid email address')
        return v.lower()

class UserResponse(BaseModel):
    id: str
    email: str
    age: int
    
    class Config:
        from_attributes = True  # For ORM models
```

**Motivation:**  
Pydantic provides automatic validation, serialization, and documentation generation.

#### 7.3 Type Hints

83. **Type hints** SHOULD be used for all function parameters and return values.

84. **mypy** SHOULD be used to check types in core modules.

**Example:**
```python
from typing import List, Optional, Dict, Any

def calculate_metrics(
    predictions: List[float],
    actuals: List[float],
    weights: Optional[Dict[str, float]] = None
) -> Dict[str, Any]:
    """Calculate evaluation metrics."""
    if weights is None:
        weights = {}
    
    # Implementation
    return {
        'mse': 0.0,
        'rmse': 0.0,
    }
```

**Motivation:**  
Type hints improve code readability and enable static type checking.

### 8. Machine Learning and LLM Development

#### 8.1 PyTorch Usage

85. **PyTorch** MUST be used only where the project explicitly requires it.

86. For general ML tasks, **scikit-learn** SHOULD be considered first.

87. Model code SHOULD be separated from training, inference, and data processing code.

**Example:**
```python
# models/transformer.py
import torch.nn as nn

class TransformerModel(nn.Module):
    def __init__(self, vocab_size: int, d_model: int):
        super().__init__()
        self.embedding = nn.Embedding(vocab_size, d_model)
        # ...

# training/trainer.py
class ModelTrainer:
    def __init__(self, model: nn.Module, config: TrainingConfig):
        self.model = model
        self.config = config
    
    def train(self, train_loader, val_loader):
        # Training logic
        pass

# inference/predictor.py
class ModelPredictor:
    def __init__(self, model_path: str):
        self.model = self.load_model(model_path)
    
    def predict(self, input_data):
        # Inference logic
        pass
```

**Motivation:**  
Separation of concerns improves testability and reusability.

#### 8.2 LangChain/LangGraph

88. LLM-based systems MUST use **LangChain** or **LangGraph**.

89. Prompt templates SHOULD be stored separately from code.

90. LLM calls SHOULD include error handling and retry logic.

**Example:**
```python
from langchain.prompts import ChatPromptTemplate
from langchain.chat_models import ChatOpenAI
from langchain.schema.runnable import RunnablePassthrough

# Separate prompt template
template = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant."),
    ("user", "{input}")
])

# Chain with error handling
def create_chain():
    llm = ChatOpenAI(temperature=0.7, max_retries=3)
    
    chain = (
        {"input": RunnablePassthrough()}
        | template
        | llm
    )
    
    return chain

# Usage with error handling
async def process_query(query: str) -> str:
    try:
        chain = create_chain()
        response = await chain.ainvoke(query)
        return response.content
    except Exception as e:
        logger.error(f"LLM call failed: {e}")
        raise
```

**Motivation:**  
LangChain provides abstractions for LLM workflows. Error handling ensures reliability.

#### 8.3 Model Versioning and Management

91. Model files SHOULD NOT be committed to git.

92. Model versioning SHOULD use a model registry or artifact storage.

93. Model metadata (architecture, training params) MUST be tracked.

**Example:**
```python
# models/registry.py
from dataclasses import dataclass
from datetime import datetime

@dataclass
class ModelMetadata:
    name: str
    version: str
    created_at: datetime
    metrics: dict[str, float]
    hyperparameters: dict[str, any]
    
class ModelRegistry:
    def save_model(self, model, metadata: ModelMetadata):
        """Save model to registry with metadata."""
        # Save to S3, MLflow, or other storage
        pass
    
    def load_model(self, name: str, version: str):
        """Load model from registry."""
        pass
```

**Motivation:**  
Model versioning enables reproducibility and rollback capability.

---

## 9. File Organization

### 9.1 Project Structure

#### Application Development (TypeScript)
94. Projects MUST use a **layered architecture** per module.

**Motivation:**  
Consistent structure reduces cognitive load and makes codebases navigable.

#### AI Engineering (Python)
95. Python projects MUST follow a **modular layered structure**.

**Recommended structure:**
```
app/
├── routers/
│   ├── predictions.py
│   └── models.py
├── services/
│   ├── prediction_service.py
│   └── model_service.py
├── models/
│   ├── schemas.py      # Pydantic models
│   └── ml_models.py    # ML model definitions
├── pipelines/
│   ├── training.py
│   └── inference.py
├── utils/
│   └── helpers.py
├── config/
│   └── settings.py
└── main.py

tests/
├── routers/
├── services/
└── pipelines/
```

**Motivation:**  
Separation of concerns improves testability and maintainability.

### 9.2 File Naming and Organization

96. Each file SHOULD have a single primary export (class, function, or component).

97. Related files SHOULD be grouped in the same directory.

98. Test files MUST be colocated with the code they test or in a parallel `__tests__` directory.

**Example:**
```
user.service.ts
user.service.spec.ts      # Colocated test

# OR

user.service.ts
__tests__/
  user.service.spec.ts    # Parallel directory
```

**Motivation:**  
Colocating tests reduces context switching and makes tests easier to find.

### 9.3 Import Organization

99. Imports MUST be organized in the following order:
    1. External libraries
    2. Internal absolute imports
    3. Relative imports

100. Import groups SHOULD be separated by blank lines.

**Example:**
```typescript
// External libraries
import React from 'react';
import { useQuery } from '@tanstack/react-query';
import axios from 'axios';

// Internal absolute imports
import { useAuth } from '@/hooks/useAuth';
import { Button } from '@/components/ui/Button';

// Relative imports
import { UserProfile } from './UserProfile';
import { formatDate } from '../utils/dateUtils';
```

**Motivation:**  
Organized imports improve readability and reduce merge conflicts.

---

## 10. Code Formatting and Layout

### 10.1 Indentation and Spacing

101. Indentation MUST be **2 spaces** for TypeScript.

102. Indentation MUST be **2 spaces** for Python (following Black's default).

103. **Tabs** are NOT ALLOWED.

**Motivation:**  
Consistent indentation improves readability across editors.

### 10.2 Line Length

104. Maximum line length MUST be **100 characters**.

105. Lines exceeding 100 characters SHOULD be broken logically.

**Example:**
```typescript
// CORRECT
const user = await userService.getUserByEmail(
    email,
    { includeProfile: true, includeOrders: true }
);

// INCORRECT
const user = await userService.getUserByEmail(email, { includeProfile: true, includeOrders: true });
```

**Motivation:**  
100 characters balances readability with modern wide screens.

### 10.3 Semicolons (TypeScript)

106. Semicolons MUST be used in TypeScript.

**Example:**
```typescript
// CORRECT
const x = 10;
const y = 20;

// INCORRECT
const x = 10
const y = 20
```

**Motivation:**  
Explicit semicolons prevent ASI (Automatic Semicolon Insertion) bugs.

### 10.4 Quotes

107. Quote style MUST follow **Prettier defaults**:
    - Double quotes for TypeScript
    - Double quotes for Python (Black default)

**Example:**
```typescript
const name = "John Doe";
import { Component } from "react";
```

```python
name = "John Doe"
from typing import List
```

**Motivation:**  
Prettier eliminates quote style debates.

### 10.5 Braces and Blocks

108. Braces MUST be used for all control structures, even single-line statements.

**Example:**
```typescript
// CORRECT
if (isValid) {
    doSomething();
}

// INCORRECT
if (isValid) doSomething();
```

**Motivation:**  
Explicit braces prevent bugs when adding statements later.

109. Opening braces MUST be on the same line as the statement.

**Example:**
```typescript
// CORRECT
if (condition) {
    // ...
}

function example() {
    // ...
}

// INCORRECT
if (condition)
{
    // ...
}
```

**Motivation:**  
Consistent brace style improves readability.

### 10.6 Blank Lines

110. Logical sections within a function SHOULD be separated by a single blank line.

111. Functions SHOULD be separated by a single blank line.

**Example:**
```typescript
function processUser(user: User): ProcessedUser {
    // Validation section
    if (!user.email) {
        throw new Error('Email required');
    }

    // Processing section
    const normalizedEmail = user.email.toLowerCase();
    const profile = getUserProfile(user.id);

    // Return section
    return {
        ...user,
        email: normalizedEmail,
        profile,
    };
}

function getUserProfile(userId: string): Profile {
    // ...
}
```

**Motivation:**  
Blank lines improve visual parsing of code structure.

### 10.7 Comments

112. Comments SHOULD explain **why**, not **what**.

**Example:**
```typescript
// INCORRECT
// Set the user name to John
const userName = "John";

// CORRECT
// Default to John for demo accounts without a configured name
const userName = "John";
```

113. Complex algorithms MUST have explanatory comments.

114. TODO comments MUST include a ticket reference or assignee.

**Example:**
```typescript
// TODO(#123): Refactor this to use the new API
// TODO(aditya): Add error handling for edge case
```

**Motivation:**  
Good comments explain intent and context, not obvious code.

---

## 11. Testing Standards

### 11.1 Test Coverage

115. Minimum test coverage MUST be **90%**.

116. Test coverage SHOULD be measured for:
     - Lines
     - Branches
     - Functions

**Configuration:**
```json
// vitest.config.ts
export default {
    test: {
        coverage: {
            provider: 'v8',
            reporter: ['text', 'json', 'html'],
            lines: 90,
            branches: 90,
            functions: 90,
            statements: 90,
        },
    },
};
```

**Motivation:**  
High coverage reduces bugs and improves confidence in refactoring.

### 11.2 Test Organization

117. Test files MUST use the naming convention: `*.spec.ts` or `*.test.ts`.

118. Test descriptions SHOULD be clear and descriptive.

**Example:**
```typescript
// user.service.spec.ts
describe('UserService', () => {
    describe('getUserById', () => {
        it('should return user when user exists', async () => {
            // Arrange
            const userId = '123';
            const expectedUser = { id: userId, name: 'John' };
            
            // Act
            const result = await userService.getUserById(userId);
            
            // Assert
            expect(result).toEqual(expectedUser);
        });

        it('should throw NotFoundException when user does not exist', async () => {
            // Arrange
            const userId = 'nonexistent';
            
            // Act & Assert
            await expect(userService.getUserById(userId))
                .rejects
                .toThrow(NotFoundException);
        });
    });
});
```

**Motivation:**  
Clear test descriptions serve as living documentation.

### 11.3 Test Structure

119. Tests SHOULD follow the **Arrange-Act-Assert** (AAA) pattern.

120. Each test SHOULD test a single behavior.

121. Test setup SHOULD use `beforeEach` for common initialization.

**Example:**
```typescript
describe('OrderService', () => {
    let orderService: OrderService;
    let mockRepository: jest.Mocked<OrderRepository>;

    beforeEach(() => {
        mockRepository = {
            findById: jest.fn(),
            save: jest.fn(),
        } as any;
        
        orderService = new OrderService(mockRepository);
    });

    it('should calculate correct total for order', () => {
        // Arrange
        const order = {
            items: [
                { price: 10, quantity: 2 },
                { price: 5, quantity: 3 },
            ],
        };

        // Act
        const total = orderService.calculateTotal(order);

        // Assert
        expect(total).toBe(35);
    });
});
```

**Motivation:**  
AAA pattern makes tests readable and maintainable.

### 11.4 Integration Tests

122. All backend services MUST have integration tests.

123. Integration tests SHOULD test complete request/response cycles.

124. Integration tests SHOULD use a test database.

**Example:**
```typescript
// user.controller.integration.spec.ts
describe('UserController (Integration)', () => {
    let app: INestApplication;

    beforeAll(async () => {
        const moduleRef = await Test.createTestingModule({
            imports: [AppModule],
        })
        .overrideProvider(DatabaseService)
        .useValue(testDatabaseService)
        .compile();

        app = moduleRef.createNestApplication();
        await app.init();
    });

    afterAll(async () => {
        await app.close();
    });

    describe('GET /users/:id', () => {
        it('should return user when authenticated', () => {
            return request(app.getHttpServer())
                .get('/users/123')
                .set('Authorization', 'Bearer valid-token')
                .expect(200)
                .expect((res) => {
                    expect(res.body).toHaveProperty('id', '123');
                    expect(res.body).toHaveProperty('name');
                });
        });

        it('should return 401 when not authenticated', () => {
            return request(app.getHttpServer())
                .get('/users/123')
                .expect(401);
        });
    });
});
```

**Motivation:**  
Integration tests catch issues that unit tests miss.

### 11.5 Test Data

125. Test data SHOULD be isolated and not depend on external state.

126. Factories or fixtures SHOULD be used for test data generation.

**Example:**
```typescript
// test/factories/user.factory.ts
export class UserFactory {
    static create(overrides?: Partial<User>): User {
        return {
            id: faker.string.uuid(),
            email: faker.internet.email(),
            name: faker.person.fullName(),
            createdAt: new Date(),
            ...overrides,
        };
    }

    static createMany(count: number): User[] {
        return Array.from({ length: count }, () => this.create());
    }
}

// Usage in tests
it('should process multiple users', () => {
    const users = UserFactory.createMany(5);
    const result = processUsers(users);
    expect(result).toHaveLength(5);
});
```

**Motivation:**  
Factories reduce test setup boilerplate and improve maintainability.

---

## 12. API Design Standards

### 12.1 RESTful Conventions

127. APIs SHOULD follow RESTful conventions.

128. HTTP methods MUST be used semantically:
     - `GET` for retrieval
     - `POST` for creation
     - `PUT` for full update
     - `PATCH` for partial update
     - `DELETE` for deletion

129. URLs SHOULD be resource-oriented, not action-oriented.

**Example:**
```
// CORRECT
GET    /users
GET    /users/123
POST   /users
PATCH  /users/123
DELETE /users/123

// INCORRECT
GET    /getUsers
POST   /createUser
POST   /updateUser
POST   /deleteUser
```

**Motivation:**  
RESTful APIs are predictable and widely understood.

### 12.2 Versioning

130. APIs SHOULD be versioned using URL path: `/api/v1/`.

131. Breaking changes MUST result in a new API version.

**Example:**
```typescript
@Controller('/api/v1/users')
export class UserControllerV1 { }

@Controller('/api/v2/users')
export class UserControllerV2 { }
```

**Motivation:**  
Versioning allows API evolution without breaking existing clients.

### 12.3 Response Format

132. All API responses MUST use consistent JSON structure.

133. Success responses SHOULD return data directly or wrapped in a `data` field.

134. Error responses MUST follow a consistent error format.

**Example:**
```typescript
// Success response
{
    "id": "123",
    "name": "John Doe",
    "email": "john@example.com"
}

// Error response
{
    "statusCode": 400,
    "message": "Validation failed",
    "errors": [
        {
            "field": "email",
            "message": "Invalid email format"
        }
    ],
    "timestamp": "2026-02-02T10:00:00Z",
    "path": "/api/v1/users"
}
```

**Motivation:**  
Consistent response format simplifies client-side error handling.

### 12.4 Pagination

135. List endpoints SHOULD support pagination.

136. Pagination SHOULD use `page` and `limit` query parameters.

137. Pagination responses MUST include metadata.

**Example:**
```typescript
// Request
GET /api/v1/users?page=2&limit=20

// Response
{
    "data": [ /* users */ ],
    "pagination": {
        "page": 2,
        "limit": 20,
        "totalItems": 150,
        "totalPages": 8,
        "hasNext": true,
        "hasPrevious": true
    }
}
```

**Motivation:**  
Pagination improves performance and user experience with large datasets.

### 12.5 Filtering and Sorting

138. List endpoints SHOULD support filtering via query parameters.

139. Sorting SHOULD use `sortBy` and `order` parameters.

**Example:**
```typescript
// Filtering
GET /api/v1/users?status=active&role=admin

// Sorting
GET /api/v1/users?sortBy=createdAt&order=desc
```

**Motivation:**  
Flexible querying reduces the need for multiple specialized endpoints.

### 12.6 OpenAPI Documentation

140. All APIs MUST have OpenAPI 3.0 documentation.

141. OpenAPI documentation MUST be generated automatically.

142. API documentation SHOULD include:
     - Description
     - Request/response examples
     - Validation rules
     - Authentication requirements

**Example (NestJS):**
```typescript
import { ApiTags, ApiOperation, ApiResponse } from '@nestjs/swagger';

@ApiTags('users')
@Controller('/api/v1/users')
export class UserController {
    @Get(':id')
    @ApiOperation({ 
        summary: 'Get user by ID',
        description: 'Retrieves a single user by their unique identifier'
    })
    @ApiResponse({ 
        status: 200, 
        description: 'User found',
        type: UserDto
    })
    @ApiResponse({ 
        status: 404, 
        description: 'User not found' 
    })
    async getUser(@Param('id') id: string): Promise<UserDto> {
        return this.userService.getUserById(id);
    }
}
```

**Example (FastAPI):**
```python
from fastapi import APIRouter, HTTPException
from pydantic import BaseModel

router = APIRouter(prefix="/api/v1/users", tags=["users"])

@router.get("/{user_id}", response_model=UserResponse)
async def get_user(user_id: str):
    """
    Get user by ID.
    
    Retrieves a single user by their unique identifier.
    
    - **user_id**: The unique identifier of the user
    
    Returns the user object if found.
    """
    user = await user_service.get_user_by_id(user_id)
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    return user
```

**Motivation:**  
Automatic documentation stays in sync with code and improves API discoverability.

---

## 13. Version Control and Collaboration

### 13.1 Git Workflow

143. The `main` branch MUST be protected and locked for direct pushes.

144. All changes MUST be made via **pull requests** (PRs).

145. PRs MUST NOT be merged with failing tests or CI checks.

**Motivation:**  
Branch protection ensures code quality and enables code review.

### 13.2 Branch Naming

146. Branch names MUST follow the convention: `<type>/<description>`.

**Valid types:**
- `feat/` - New features
- `fix/` - Bug fixes
- `refactor/` - Code refactoring
- `chore/` - Maintenance tasks
- `docs/` - Documentation updates
- `test/` - Test additions/updates

**Examples:**
```
feat/user-authentication
fix/payment-calculation-error
refactor/user-service
chore/update-dependencies
docs/api-documentation
test/user-controller-integration
```

**Motivation:**  
Consistent branch naming improves repository organization and automation.

### 13.3 Commit Messages

147. Commit messages MUST follow the **Conventional Commits** specification.

**Format:**
```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation
- `style`: Formatting, missing semicolons, etc.
- `refactor`: Code restructuring
- `perf`: Performance improvement
- `test`: Adding tests
- `chore`: Maintenance tasks

**Examples:**
```
feat(auth): add JWT authentication

Implements JWT-based authentication with refresh tokens.
Includes login, logout, and token refresh endpoints.

Closes #123

fix(orders): correct tax calculation for international orders

The tax calculation was not accounting for VAT properly.
This fix ensures correct tax rates are applied based on
the customer's country.

Fixes #456

docs(api): update OpenAPI documentation for user endpoints

chore(deps): upgrade React to v18.2.0
```

**Motivation:**  
Conventional commits enable automatic changelog generation and semantic versioning.

### 13.4 Pull Request Requirements

148. PRs MUST include:
     - Clear description of changes
     - Link to related issue/ticket
     - Screenshots for UI changes
     - Test evidence (coverage, passing tests)

149. PRs SHOULD be small and focused on a single concern.

150. PRs MUST be reviewed by at least one other developer before merging.

**PR Template Example:**
```markdown
## Description
Brief description of what this PR does.

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Related Issue
Closes #123

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests added/updated
- [ ] Manual testing performed

## Screenshots (if applicable)
[Add screenshots for UI changes]

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex logic
- [ ] Documentation updated
- [ ] Tests pass locally
```

**Motivation:**  
Structured PRs improve review quality and maintain project documentation.

### 13.5 Merge Strategy

151. The recommended merge strategy is:
     1. **Squash** for feature branches
     2. **Rebase** for linear history
     3. **Merge** for long-running branches

152. Merge conflicts SHOULD be resolved collaboratively with the PR reviewer.

**Motivation:**  
Consistent merge strategy keeps history clean and traceable.

### 13.6 Code Review

153. Code reviews SHOULD focus on:
     - Correctness
     - Test coverage
     - Code style adherence
     - Security issues
     - Performance concerns

154. Review feedback SHOULD be constructive and specific.

155. Reviewers SHOULD approve PRs only when satisfied with the quality.

156. Authors SHOULD respond to all review comments.

**Example Review Comments:**
```
✅ Good: "This function could benefit from early returns to reduce nesting. 
Consider checking for error conditions first, then handling the success case."

❌ Bad: "This is confusing."

✅ Good: "There's a potential SQL injection vulnerability here. 
We should use parameterized queries instead."

❌ Bad: "Security issue."
```

**Motivation:**  
Effective code review improves code quality and knowledge sharing.

---

## 14. Security and Configuration

### 14.1 Environment Variables

157. Sensitive configuration MUST use environment variables.

158. Environment variables MUST NEVER be committed to version control.

159. `.env` files MUST be added to `.gitignore`.

160. `.env.example` SHOULD be provided as a template.

**Example:**
```bash
# .env.example
DATABASE_URL=postgresql://user:password@localhost:5432/dbname
JWT_SECRET=your-secret-key-here
API_KEY=your-api-key-here
```

**Motivation:**  
Environment variables keep secrets out of source code.

### 14.2 Secrets Management

161. Production/Staging secrets MUST NEVER be commited to version control.

162. API keys and tokens MUST be rotated regularly.

163. Developers MUST NOT share secrets via email, or unencrypted channels.

**Motivation:**  
Proper secrets management prevents security breaches.

### 14.3 Input Validation

164. All user input MUST be validated.

165. Validation SHOULD happen at multiple layers (client, controller, service).

166. SQL queries MUST use parameterized statements to prevent SQL injection.

**Example:**
```typescript
// Validation at controller layer
@Post()
async createUser(@Body() createUserDto: CreateUserDto) {
    // DTO validation happens automatically via class-validator
    return this.userService.createUser(createUserDto);
}

// Additional validation in service layer
async createUser(dto: CreateUserDto) {
    if (await this.userExists(dto.email)) {
        throw new ConflictException('User already exists');
    }
    // ...
}
```

**Motivation:**  
Defense in depth prevents security vulnerabilities.

### 14.4 Authentication and Authorization

167. Authentication tokens SHOULD have expiration times.

168. Sensitive endpoints MUST require authentication.

169. Authorization checks MUST happen on the server side.

170. Passwords MUST be hashed using a strong algorithm (bcrypt, Argon2).

**Example:**
```typescript
import * as bcrypt from 'bcrypt';

async function hashPassword(password: string): Promise<string> {
    const salt = await bcrypt.genSalt(10);
    return bcrypt.hash(password, salt);
}

async function verifyPassword(password: string, hash: string): Promise<boolean> {
    return bcrypt.compare(password, hash);
}
```

**Motivation:**  
Proper authentication and authorization prevent unauthorized access.

---

## 15. Documentation

### 15.1 Code Documentation

171. Public APIs MUST have documentation comments.

172. Complex logic SHOULD be documented with inline comments.

173. Documentation SHOULD be kept up-to-date with code changes.

**TypeScript Example (JSDoc):**
```typescript
/**
 * Calculates the total price of an order including taxes and discounts.
 * 
 * @param items - Array of order items
 * @param taxRate - Tax rate as a decimal (e.g., 0.08 for 8%)
 * @param discountCode - Optional discount code
 * @returns The total price after taxes and discounts
 * @throws {InvalidDiscountError} If the discount code is invalid
 * 
 * @example
 * ```typescript
 * const total = calculateTotal(
 *   [{ price: 100, quantity: 2 }],
 *   0.08,
 *   'SAVE10'
 * );
 * ```
 */
function calculateTotal(
    items: OrderItem[],
    taxRate: number,
    discountCode?: string
): number {
    // Implementation
}
```

**Python Example (Docstring):**
```python
def calculate_metrics(predictions: List[float], actuals: List[float]) -> Dict[str, float]:
    """
    Calculate evaluation metrics for model predictions.
    
    Args:
        predictions: List of predicted values
        actuals: List of actual values
        
    Returns:
        Dictionary containing:
            - mse: Mean squared error
            - rmse: Root mean squared error
            - mae: Mean absolute error
            
    Raises:
        ValueError: If predictions and actuals have different lengths
        
    Example:
        >>> metrics = calculate_metrics([1.0, 2.0, 3.0], [1.1, 2.1, 2.9])
        >>> print(metrics['rmse'])
        0.1
    """
    if len(predictions) != len(actuals):
        raise ValueError("Predictions and actuals must have the same length")
    
    # Implementation
    return {'mse': 0.0, 'rmse': 0.0, 'mae': 0.0}
```

**Motivation:**  
Good documentation reduces onboarding time and prevents misuse of APIs.

### 15.2 README Files

174. Every project MUST have a `README.md` file.

175. README files SHOULD include:
     - Project description
     - Installation instructions
     - Usage examples
     - Configuration options
     - Testing instructions
     - Contributing guidelines

**Example README structure:**
```markdown
# Project Name

Brief description of what this project does.

## Prerequisites

- Node.js 18+
- PostgreSQL 14+

## Installation

\`\`\`bash
npm install
cp .env.example .env
npm run db:migrate
\`\`\`

## Usage

\`\`\`bash
npm run dev
\`\`\`

## Testing

\`\`\`bash
npm test
npm run test:coverage
\`\`\`

## API Documentation

API documentation is available at `/api/docs` when running the server.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.
```

**Motivation:**  
READMEs are the entry point for new developers and should be comprehensive.


---

## Appendix A: Quick Reference

### Must-Have Checklist for New Projects

**Application Development (TypeScript):**
- [ ] NestJS configured
- [ ] ESLint configured
- [ ] Prettier configured
- [ ] Vitest configured
- [ ] Husky pre-commit hooks configured
- [ ] TanStack Query configured
- [ ] CASL authorization configured
- [ ] OpenAPI documentation enabled
- [ ] Environment variables template (`.env.example`)
- [ ] README with installation instructions
- [ ] CI/CD pipeline configured

**AI Engineering (Python):**
- [ ] FastAPI configured
- [ ] Black configured
- [ ] uv package manager configured
- [ ] Pydantic models defined
- [ ] OpenAPI documentation enabled
- [ ] Environment variables template (`.env.example`)
- [ ] README with installation instructions
- [ ] Model versioning strategy defined
- [ ] CI/CD pipeline configured


---

## Appendix B: Resources

### Official Documentation
- [TypeScript Documentation](https://www.typescriptlang.org/docs/)
- [NestJS Documentation](https://docs.nestjs.com/)
- [React Documentation](https://react.dev/)
- [TanStack Query](https://tanstack.com/query/latest)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [Pydantic Documentation](https://docs.pydantic.dev/)
- [PEP 8 Style Guide](https://pep8.org/)

### Tools
- [ESLint](https://eslint.org/)
- [Prettier](https://prettier.io/)
- [Black](https://black.readthedocs.io/)
- [Vitest](https://vitest.dev/)
- [uv](https://github.com/astral-sh/uv)


---

**End of Document**
