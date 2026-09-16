1.Error- Jenkins indexes the dev branch but skips running the build, 
         reporting: "No Jenkinsfile found in repository at the expected path."

2.Root cause:- Dev branch is missing may be due to wrong extension or different name
3.Fix:- cheack the name and fix it
4.verify:- Run Scan multibranch pipline now and that it shows Jenkinsfile was found without skipping 
