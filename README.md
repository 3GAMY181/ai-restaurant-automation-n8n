[README.md](https://github.com/user-attachments/files/32478147/README.md)
# 🤖 AI Restaurant Customer Service & Order Automation

An AI-powered restaurant automation workflow built with **n8n**, **Telegram**, **LLM**, **Supabase Vector Store**, **OpenAI Embeddings**, and **Google Sheets**.

The system automates customer communication, answers restaurant questions using a Knowledge Base, understands incoming orders, and connects the AI Agent to operational tools.

## 🚀 Project Overview

A customer sends a message through Telegram. The workflow analyzes the message, determines its intent, routes it to the appropriate path, and uses an AI Agent to generate a response or interact with connected tools.

### Main capabilities

- 💬 Automated Telegram customer support
- 🧠 LLM-based intent classification
- 📚 RAG / Knowledge Base using Supabase Vector Store
- 🔎 Semantic search using embeddings
- 🤖 AI Agent for customer conversations
- 🧾 Order-related automation
- 📊 Google Sheets integration
- 🧮 Code Tool for custom business logic
- 🧠 Conversation memory
- 🔄 Automated Telegram responses

## 🏗️ Workflow Architecture

```text
Customer
   │
   ▼
Telegram
   │
   ▼
Telegram Trigger
   │
   ▼
Basic LLM Chain
   │
   ├── OpenRouter Chat Model
   └── Structured Output Parser
   │
   ▼
Switch
   │
   ├── General
   ├── Order
   └── Question
          │
          ▼
       AI Agent
          │
    ┌─────┼───────────────┐
    │     │               │
    ▼     ▼               ▼
Supabase  Google Sheets  Code Tool
Vector    Tools
Store
    │
    ▼
OpenAI Embeddings
    │
    ▼
AI Response
    │
    ▼
Telegram
    │
    ▼
Customer
```

## 🔄 How It Works

### 1. Telegram Trigger

The workflow starts when a customer sends a Telegram message.

Example:

```text
عايز بيتزا مارجريتا
```

The Telegram Trigger receives the message and passes it to the AI processing pipeline.

### 2. Basic LLM Chain

The incoming message is processed by a Basic LLM Chain using an **OpenRouter Chat Model** to understand the customer's message and determine its general intent.

Typical categories:

- `General`
- `Order`
- `Question`

### 3. Structured Output Parser

The LLM output is converted into a structured format so n8n can reliably use the result.

Example:

```json
{
  "type": "Order"
}
```

### 4. Switch

The Switch node routes the message according to its detected intent.

**General** — general conversation, such as `مساء الخير`.

**Order** — customers who want to place or modify an order, such as `عايز 2 بيتزا مارجريتا`.

**Question** — restaurant-related questions, such as `سعر البرجر الكلاسيك كام؟`.

## 🤖 AI Agent

The AI Agent is the main decision-making component. It receives customer context and can use connected tools to retrieve information or perform operations.

Connected components:

- OpenRouter Chat Model
- Simple Memory
- Supabase Vector Store
- Google Sheets tools
- Code Tool

## 🧠 Conversation Memory

The workflow uses **Simple Memory** to maintain conversation context.

Example:

```text
Customer:
عايز بيتزا مارجريتا

Customer:
اتنين
```

The Agent can use the previous message context to understand that the customer wants 2 Margherita pizzas.

## 📚 Knowledge Base / RAG

The AI Agent is connected to a **Supabase Vector Store** containing the restaurant Knowledge Base.

The Knowledge Base includes:

- Restaurant menu
- Food prices
- Ingredients
- Preparation times
- Delivery policies
- Payment methods
- Cancellation rules
- Customer complaint procedures
- Allergy-related information
- Ready-made customer responses
- AI Agent operating rules

The Knowledge Base is maintained as text/Markdown so it can be chunked and embedded for semantic retrieval.

## 🔎 Embeddings

The Supabase Vector Store uses **OpenAI Embeddings** to transform Knowledge Base content into vector representations for semantic search.

For example, a customer may ask:

```text
البيتزا بتاعتكم بكام؟
```

The Agent can retrieve relevant menu information from the Knowledge Base even when the wording does not exactly match the stored text.

## 🍕 Current Menu

### Burgers & Sandwiches

- Classic Beef Burger — 140 EGP
- Double Bacon Burger — 180 EGP
- Chicken Crispy Sandwich — 130 EGP
- Beef or Chicken Fajita Sandwich — 145 EGP

### Pizza

- Margherita Pizza — 110 EGP
- Pepperoni Pizza — 150 EGP
- Chicken BBQ Pizza — 160 EGP
- Seafood Pizza — 195 EGP

### Main Meals

- Grilled Chicken Meal — 170 EGP
- Beef Steak with White Sauce — 260 EGP
- Macaroni Bechamel — 95 EGP
- Chicken White Sauce Pasta — 140 EGP

### Appetizers & Sides

- French Fries — 45 EGP
- Mozzarella Sticks, 6 pieces — 65 EGP
- Crispy Onion Rings — 40 EGP
- Chicken Caesar Salad — 85 EGP

### Desserts

- Fruit Cheesecake — 75 EGP
- Molten Cake — 85 EGP
- Om Ali with Nuts — 60 EGP

### Drinks

- Canned Soft Drinks — 25 EGP
- Fresh Juice — 45 EGP
- Mineral Water — 15 EGP large / 25 EGP small

> Menu data is based on the current Knowledge Base and should be updated whenever the restaurant changes its menu or prices.

## 📊 Google Sheets Tools

### Order Sheet

Used for accessing order-related information.

### Write Update

Used for updating order information.

### Cancelled Sheets

Used for handling cancelled-order records.

These tools allow the AI Agent to move beyond simple Q&A and interact with operational data.

## 🧩 Code Tool

The AI Agent also has access to a Code Tool for custom business logic such as:

- Calculating order totals
- Processing quantities
- Transforming data
- Validating information
- Implementing custom restaurant rules

## 📱 Example Customer Flow

Customer:

```text
عايز 2 بيتزا مارجريتا
```

Workflow:

```text
Telegram
   ↓
Intent Classification
   ↓
Order
   ↓
AI Agent
   ↓
Knowledge Base
   ↓
Margherita Pizza = 110 EGP
   ↓
2 × 110 = 220 EGP
   ↓
Order Tool
   ↓
Telegram Response
```

Example response:

```text
تمام، طلبك 2 بيتزا مارجريتا،
الإجمالي 220 جنيه.

هل تريد تأكيد الطلب؟
```

## 🗂️ Repository Structure

```text
ai-restaurant-automation-n8n/
│
├── README.md
├── workflow/
│   └── restaurant-ai-agent.json
├── knowledge-base/
│   └── restaurant-knowledge-base.md
├── docs/
│   └── architecture.png
└── screenshots/
    └── n8n-workflow.png
```

## 🔐 Security

**Never commit secrets to GitHub.**

Do not upload:

- Telegram Bot Token
- OpenRouter API Key
- OpenAI API Key
- Supabase Service Role Key
- Google credentials
- Database passwords
- Private environment variables

Use n8n Credentials or environment variables instead.

Before publishing the workflow JSON, verify that no sensitive credential values are embedded inside the exported workflow.

## 🛠️ Setup

### Requirements

- n8n
- Telegram Bot
- OpenRouter API access
- OpenAI API access for embeddings
- Supabase project
- Supabase Vector Store
- Google Sheets
- Google credentials configured in n8n

### Basic Setup

1. Import the n8n workflow JSON.
2. Configure Telegram credentials.
3. Configure the OpenRouter Chat Model.
4. Configure OpenAI Embeddings.
5. Create/configure the Supabase Vector Store.
6. Upload and index the restaurant Knowledge Base.
7. Configure Google Sheets credentials.
8. Connect the required Google Sheets tools.
9. Verify the AI Agent instructions.
10. Test the workflow with Telegram.
11. Activate the workflow.

## 📥 Knowledge Base

Keep the Knowledge Base separate from the workflow so restaurant information can be updated without redesigning the automation.

Recommended location:

```text
knowledge-base/
└── restaurant-knowledge-base.md
```

Update and re-index the Knowledge Base whenever the restaurant changes menu items, prices, ingredients, delivery rules, payment methods, preparation times, or complaint policies.

## 🧪 Testing Examples

### General Message

```text
مساء الخير
```

Expected intent: `General`

### Menu Question

```text
سعر بيتزا المارجريتا كام؟
```

Expected intent: `Question`

Expected result:

```text
بيتزا مارجريتا
السعر: 110 جنيه
```

### Order

```text
عايز 2 بيتزا مارجريتا
```

Expected intent: `Order`

### Missing Item

If a customer asks for an item that is not in the menu, the Agent should not invent a product. It should state that the item is unavailable and suggest an available alternative when appropriate.

## 🎯 Project Goals

The project combines:

```text
n8n
+
LLM
+
RAG
+
Supabase
+
Telegram
+
Google Sheets
```

to create an AI-powered restaurant customer-service and order-automation foundation.

## 🚧 Future Improvements

Potential extensions include:

- WhatsApp integration
- Online payment integration
- Delivery tracking
- Automatic order status notifications
- Inventory automation
- Low-stock alerts
- Customer profiles
- CRM integration
- Sales analytics
- Automated daily reports
- Multi-branch restaurant support
- Voice-based ordering
- Advanced order validation
- Automated customer follow-ups

## 👨‍💻 Author

## Workflow
<img width="1920" height="850" alt="image" src="https://github.com/user-attachments/assets/b673c0e4-9a99-4d6e-83ae-b6242f25d6c8" />


**Ahmed Elagamy**  
AI Automation Specialist & Information Systems Engineer

GitHub: `https://github.com/3GAMY181`


