@Library("lib2") _

echo 'Loading pipeline definition'
        	Yaml parser = new Yaml()
  	  	    Map configParser = parser.load(new File(pwd() + '/devops.yaml').text)
			print configParser

standardPipeline {
        projectName = ${configParser.name}
        serverDomain = ${configParser.template}
}
