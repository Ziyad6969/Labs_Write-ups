Solution – Cool Name Effect
===========================

> Spoiler warning: This file contains the full solution.

### 1\. Initial Observation

The page takes user input and displays it in a curved arch-like format.The lab mentions improper filtering, suggesting a possible XSS vulnerability.

### 2\. Inspecting the Source Code

Pressing **F12** shows a large obfuscated JavaScript block:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   eval(function(p,a,c,k,e,d){...})   `

### 3\. Deobfuscation

Using an online deobfuscation tool, the code is readable.It defines a function called newAlert that overrides:

*   window.alert
    
*   window.confirm
    
*   window.prompt
    

Inside, an array is defined:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   var z = ['y','o','u','r',' ','f','l','a','g',' ','i','s',':'];   `

Since window.alert is overwritten, calling alert() will actually execute newAlert().

### 4\. Testing for XSS

Injecting the payload:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   alert(1)   `

was replaced with:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   alert(1)   `

The lowercase script tag is blocked.

### 5\. Filter Bypass

Using a case variation:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   alert(1)   `

worked successfully, bypassing the filter.

### 6\. Flag Retrieval

Calling:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   alert()   `

executes the custom alert function and reveals the flag.The challenge is completed successfully.

### Takeaways

*   Simple string replacement is not secure.
    
*   Case-sensitive filters can be bypassed.
    
*   Overriding native JS functions doesn’t prevent XSS.
    
*   Client-side filtering is unreliable for security.
