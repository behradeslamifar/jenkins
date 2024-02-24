# Pipelines

## Pipeline Types
- Declarative Pipeline 
- Scripted Pipeline
- Plain Groovy

### Declarative versus Scripted Pipeline
Declarative and Scripted Pipelines are constructed fundamentally differently. Declarative Pipeline is a more recent feature of Jenkins Pipeline which:
- provides richer syntactical features over Scripted Pipeline syntax, and
- is designed to make writing and reading Pipeline code easier.
- also read this [jenkins.io: Differences from plain Groovy and Syntax Comparison](https://www.jenkins.io/doc/book/pipeline/syntax/#differences-from-plain-groovy)

### Declarative pipeline

```
pipeline {
    agent any 
    stages {
        stage('Build') { 
            steps {
                // 
            }
        }
        stage('Test') { 
            steps {
                // 
            }
        }
        stage('Deploy') { 
            steps {
                // 
            }
        }
    }
}
```

### Scripted Pipeline

```
node {  
    stage('Build') { 
        // 
    }
    stage('Test') { 
        // 
    }
    stage('Deploy') { 
        // 
    }
}
```

## Resources
- [jenkins.io: Pipeline](https://www.jenkins.io/doc/book/pipeline/)


## Resource
- [jenkins.io: Getting started with Pipeline](https://www.jenkins.io/doc/book/pipeline/getting-started/)
- [github.io: Pipeline Examples](https://github.com/jenkinsci/pipeline-examples)
- [jenkins.io: Pipeline Examples (old example)](https://www.jenkins.io/doc/pipeline/examples/)
