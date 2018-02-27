# tools:sample

![header](https://i.imgur.com/BQcBTXZ.png)

This repo is a collection of sample data that you can use in your project.
It also contains examples on how to improve preview of your layout.

Usage
-------

Add following code into your `build.gradle`.

    clean.doFirst {
        println "cleanSamples"
        def samplesDir = new File(projectDir.absolutePath, "sampledata")
        if (samplesDir.exists()) {
            samplesDir.deleteDir()
        }
    }
        
    preBuild.doFirst {
        println "fetchSamplesStarted"
        def samplesDir = new File(projectDir.absolutePath, "sampledata")
        if (samplesDir.exists()) {
            println "samples dir already exists"
            return
        }
        samplesDir.mkdir()

        def files = ["lotr.json", "starwars.json", "hitchhikers.json", "developers.json"]
        for(fileName in files) {
            def names = new File(samplesDir, fileName)

            new URL("https://raw.githubusercontent.com/vlad1m1r990/tools-sample/master/sampledata/$fileName").withInputStream { i ->
                names.withOutputStream {
                   it << i
                }
            }
            println "fetchSample: $fileName"
        }
        println "fetchSamplesEnded"
    }

After you rebuild your project, you will have files defined in `files` array in your `sampledata` folder.
Now you can use the sample data in your layout like this:

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        tools:text="@sample/lotr.json/characters/name" />


Credits
-------

+ [Vladimir Jovanovic](https://github.com/vlad1m1r990)

License
-------

    Copyright 2018 Vladimir Jovanovic

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

         http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.