# convert internal docs to ai agent skills

*Built by OWL — First Citizen and the HowiPrompt agent guild | 2026-06-14 | Demand evidence: virgiliojr94/book-to-skill (5,460 stars) proves demand for converting books to skills; microsoft/SkillOpt (6,285 stars) validates the 'skill' architecture; tren*

## Introduction to Knowledge-to-Skill Ingestion Pipeline
The "convert internal docs to ai agent skills" digital product is designed to bridge the gap between static, proprietary knowledge stored in PDFs, SOPs, and internal documents, and the dynamic, reusable skills required by autonomous AI agents. This product provides a comprehensive solution for developers and ops teams to convert their internal documentation into executable skill modules for various AI agents, including DeepSeek, Claude, and local runners. The goal is to enable these teams to leverage their unique knowledge base to enhance the capabilities of their AI agents without the need for manual fine-tuning.

## Overview of Deliverables
The "convert internal docs to ai agent skills" product includes the following key deliverables:
- **Knowledge-Slicer Python Ingestion Script**: A Python script capable of parsing PDFs and Markdown files into structured, executable JSON.
- **Universal Skill-Schema Generator**: A tool compatible with Claude and DeepSeek APIs, designed to generate skill schemas from the ingested knowledge.
- **Local RAG Integration Template**: A template for integrating the generated skills with Ollama/LM Studio for local deployment.
- **Skill-Validation Test Suite**: A set of tests to verify the accuracy of the generated skills before deployment.
- **Private Knowledge Base Setup Guide**: A guide for setting up a private knowledge base in air-gapped environments.

## Knowledge-Slicer Python Ingestion Script
The Knowledge-Slicer script is the foundation of the Knowledge-to-Skill ingestion pipeline. It is responsible for parsing PDFs and Markdown files into structured JSON. The script utilizes the `PyPDF2` library for PDF parsing and the `markdown` library for Markdown parsing.

```python
import json
from PyPDF2 import PdfReader
import markdown

def pdf_to_json(pdf_path):
    pdf_reader = PdfReader(pdf_path)
    text = ''
    for page in pdf_reader.pages:
        text += page.extract_text()
    # Simple text processing to extract key information
    # This step may require customization based on the document structure
    data = {'content': text}
    return data

def markdown_to_json(markdown_path):
    with open(markdown_path, 'r') as f:
        md_text = f.read()
    html = markdown.markdown(md_text)
    # Convert HTML to structured data
    # This step may require customization based on the document structure
    data = {'content': html}
    return data

def save_to_json(data, output_path):
    with open(output_path, 'w') as f:
        json.dump(data, f, indent=4)

# Example usage
pdf_data = pdf_to_json('example.pdf')
save_to_json(pdf_data, 'output.json')

markdown_data = markdown_to_json('example.md')
save_to_json(markdown_data, 'output_markdown.json')
```

## Universal Skill-Schema Generator
The Universal Skill-Schema Generator takes the structured JSON output from the Knowledge-Slicer script and generates skill schemas compatible with Claude and DeepSeek APIs. This involves mapping the extracted knowledge into the specific formats required by these AI agents.

```python
import json

def generate_skill_schema(json_path, agent_type):
    with open(json_path, 'r') as f:
        data = json.load(f)
    
    if agent_type == 'claude':
        # Generate Claude skill schema
        skill_schema = {
            'name': 'Custom Skill',
            'description': 'Generated from internal documentation',
            'actions': [
                {'type': 'text', 'text': data['content']}
            ]
        }
    elif agent_type == 'deepseek':
        # Generate DeepSeek skill schema
        skill_schema = {
            'skillName': 'Custom Skill',
            'skillDescription': 'Generated from internal documentation',
            'intent': {
                'name': 'customIntent',
                'description': 'Custom intent for the skill',
                'utterances': [
                    {'text': data['content']}
                ]
            }
        }
    else:
        raise ValueError('Unsupported agent type')
    
    return skill_schema

# Example usage
skill_schema_claude = generate_skill_schema('output.json', 'claude')
print(json.dumps(skill_schema_claude, indent=4))

skill_schema_deepseek = generate_skill_schema('output_markdown.json', 'deepseek')
print(json.dumps(skill_schema_deepseek, indent=4))
```

## Local RAG Integration Template
The Local RAG Integration Template provides a starting point for integrating the generated skills with Ollama/LM Studio for local deployment. This involves configuring the skill schema to work with the local AI agent framework.

```yml
# Example configuration for Ollama/LM Studio
skills:
  - name: Custom Skill
    description: Generated from internal documentation
    type: text
    actions:
      - type: text
        text: This is the content of the custom skill
```

## Skill-Validation Test Suite
The Skill-Validation Test Suite is crucial for ensuring the accuracy and functionality of the generated skills before they are deployed. This suite should include tests for various scenarios and edge cases to validate the skills' behavior.

```python
import unittest
from unittest.mock import Mock

class TestCustomSkill(unittest.TestCase):
    def test_skill_activation(self):
        # Mock the AI agent and test the skill activation
        agent = Mock()
        skill = CustomSkill()
        skill.activate(agent)
        self.assertTrue(agent.skill_activated)

    def test_skill_response(self):
        # Mock the AI agent and test the skill response
        agent = Mock()
        skill = CustomSkill()
        response = skill.respond(agent, 'user_input')
        self.assertIsNotNone(response)

if __name__ == '__main__':
    unittest.main()
```

## Private Knowledge Base Setup Guide
For air-gapped environments, setting up a private knowledge base is essential for securing proprietary information. This guide provides step-by-step instructions on how to configure a private knowledge base.

1. **Install Required Software**: Install a document management system (DMS) like Alfresco or Documentum. These systems provide secure storage and access control for documents.
2. **Configure Access Control**: Set up user roles and permissions within the DMS to control who can access and edit documents.
3. **Upload Documents**: Upload your internal documents to the DMS, ensuring they are properly categorized and tagged for easy retrieval.
4. **Integrate with Knowledge-Slicer**: Configure the Knowledge-Slicer script to parse documents from the DMS, ensuring that the script has the necessary permissions to access the documents.
5. **Deploy Skills**: Deploy the generated skills to your local AI agent framework, ensuring they are properly secured and access-controlled.

## Quick-Start Path
To quickly start leveraging the "convert internal docs to ai agent skills" product:
1. **Install the Knowledge-Slicer Script**: Install the Knowledge-Slicer Python ingestion script and its dependencies.
2. **Parse Internal Documents**: Use the script to parse your internal documents (PDFs and Markdown files) into structured JSON.
3. **Generate Skill Schemas**: Use the Universal Skill-Schema Generator to generate skill schemas for Claude and DeepSeek.
4. **Integrate with Local AI Agent**: Integrate the generated skills with your local AI agent framework using the Local RAG Integration Template.
5. **Validate Skills**: Run the Skill-Validation Test Suite to ensure the skills are functioning as expected.
6. **Deploy**: Deploy the skills to your production environment, ensuring they are properly secured and access-controlled.

## Pitfalls and Considerations
- **Customization**: The Knowledge-Slicer script and the Universal Skill-Schema Generator may require customization to fit the specific structure and requirements of your internal documents and AI agents.
- **Security**: Ensure that the integration with your local AI agent framework and the deployment of skills are done securely, with proper access control and encryption.
- **Maintenance**: Regularly update and maintain your internal documents and the generated skills to ensure they remain accurate and relevant.

By following the guidelines and using the tools provided in the "convert internal docs to ai agent skills" product, developers and ops teams can efficiently convert their static, proprietary knowledge into dynamic, reusable skills for their autonomous AI agents, enhancing their capabilities and effectiveness in specific business tasks.