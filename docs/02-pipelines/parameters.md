# Parameters

## Parameter Types
#### Build in Parameter
- booleanParam
- string 
- choice
- credentials
- file
- text
- password

#### Parameter plugins
- hidden
- activeChoice
- reactiveChoice
- reactiveReferenceParameter

## Access to Parameters
If you define in UI or calling from another pipeline
```
pipeline {
  agent any
  stages {
    stage('Build')
      steps {
        echo "Version is ${params.Version}"
      }
    }
}
```

Or define parametres in pipeline
```pipeline {                      
  agent any                     
  parameters {
    string(name: 'Version', defaultValue: "NA", description: 'Application Version')
  stages {                      
    stage('Build')              
      steps {                   
        echo "Version is $Version"
      }                         
    }                           
}                               
```

## Define all Types of Parameter in Pipeline
#### Simple build Parameter type
```
pipeline {
  agent any

  parameters {
    booleanParam(name: 'Answer', defaultValue: "false", description: 'description')
    string(name: 'Version', defaultValue: "NA", description: 'Application Version')
    choice(name: 'Region', choices: ['','us','eu'], description: 'Choose region')
    credentials(name: 'ssh_key', description: 'Choose ssh key from credentials list')
    file(name: "Manifest_file", description: "Upload your manifest file here")
    text(name: "Config", defaultValue: "default config", description: "Update your configuration")
    password(name: "Token", description: "Enter API token")
  }

  stages {
    stage('Show Value') {
      steps {
        echo "App version is: ${params.Version}"
      }
    }
  }
}

#### Parameters Plugins
```groovy
pipeline {
  agent any

  parameters {
    hidden(name: 'Hidden_flag', defaultValue: 'true', description: 'Internal pipeline flag')

    // Active Choice Parameter
    // Choice Types: PT_SINGLE_SELECT, PT_MULTI_SELECT, PT_CHECKBOX, PT_RADIO
    activeChoice(
        name: 'Continent', 
        description: 'Select one',
        choiceType: 'PT_SINGLE_SELECT',
        script: groovyScript( 
            script: [sandbox: true, script: '''
                return ['','America','Europ']
            '''],
            fallbackScript: [sandbox: true, script: 'return ["error"]']
        )
    )

    // Active Choinces Reactive Parameters
    // Choice Types: PT_SINGLE_SELECT, PT_MULTI_SELECT, PT_CHECKBOX, PT_RADIO
    reactiveChoice(
        name: 'Environment', 
        description: 'Select one',
        choiceType: 'PT_SINGLE_SELECT',
        referencedParameters: 'Region',
        filterLength: 1,
        filterable: false,
        script: groovyScript( 
            script: [sandbox: true, script: '''
                if (Region == 'us') {
                    return ['United State']
                } else {
                    return ['Please Choose US']
                }
            '''],
            fallbackScript: [sandbox: true, script: 'return ["error"]']
        )
    )

    // Active Choince Reactive Reference Parameters - Renders as HTML - shows info panel based on ...
    // Choice type can be, ET_ORDERED_LIST, ET_UNORDERED_LIST, ET_FORMATTED_HIDDEN_HTML, ET_FORMATTED_HTML
    reactiveReferenceParameter(
        name: 'Environment', 
        description: 'Select one',
        choiceType: 'ET_FORMATTED_HTML',
        referencedParameters: 'Region',
        filterLength: 1,
        filterable: false,
        script: groovyScript( 
            script: [sandbox: true, script: '''
              if (Region == 'us') {
                  return "<div><b>United State</div>"
              } else {
                  return "<input type=text placeholder="Select region" name="value" style="width:300px" />
              }
            '''],
            fallbackScript: [sandbox: true, script: 'return ["error"]']
        )
    )
    
  }

  stages {
    stage('Show Value') {
      steps {
        echo "Hidden_flag=${params.Hidden_flag}"
      }
    }
  }
}
```

