# Example of failure for spotbugs gradle plugin and Gradle 4.8-rc-1

To reproduce you can call `./gradlew clean build --stacktrace`

You will see output like:

```
Caused by: java.lang.AbstractMethodError: org.gradle.api.plugins.quality.internal.AbstractCodeQualityPlugin.configureConfiguration(Lorg/gradle/api/artifacts/Configuration;)V
        at org.gradle.api.plugins.quality.internal.AbstractCodeQualityPlugin.createConfigurations(AbstractCodeQualityPlugin.java:106)
        at org.gradle.api.plugins.quality.internal.AbstractCodeQualityPlugin.apply(AbstractCodeQualityPlugin.java:57)
        at org.gradle.api.plugins.quality.internal.AbstractCodeQualityPlugin.apply(AbstractCodeQualityPlugin.java:42)
        at org.gradle.api.internal.plugins.ImperativeOnlyPluginTarget.applyImperative(ImperativeOnlyPluginTarget.java:42)
        at org.gradle.api.internal.plugins.RuleBasedPluginTarget.applyImperative(RuleBasedPluginTarget.java:50)
        at org.gradle.api.internal.plugins.DefaultPluginManager.addPlugin(DefaultPluginManager.java:163)
        at org.gradle.api.internal.plugins.DefaultPluginManager.access$200(DefaultPluginManager.java:46)
        at org.gradle.api.internal.plugins.DefaultPluginManager$AddPluginBuildOperation.run(DefaultPluginManager.java:251)
        at org.gradle.internal.operations.DefaultBuildOperationExecutor$RunnableBuildOperationWorker.execute(DefaultBuildOperationExecutor.java:317)
        at org.gradle.internal.operations.DefaultBuildOperationExecutor$RunnableBuildOperationWorker.execute(DefaultBuildOperationExecutor.java:309)
        at org.gradle.internal.operations.DefaultBuildOperationExecutor.execute(DefaultBuildOperationExecutor.java:185)
        at org.gradle.internal.operations.DefaultBuildOperationExecutor.run(DefaultBuildOperationExecutor.java:97)
        at org.gradle.internal.operations.DelegatingBuildOperationExecutor.run(DelegatingBuildOperationExecutor.java:31)
        at org.gradle.api.internal.plugins.DefaultPluginManager.doApply(DefaultPluginManager.java:143)
        at org.gradle.api.internal.plugins.DefaultPluginManager.apply(DefaultPluginManager.java:124)
        at org.gradle.api.internal.plugins.DefaultObjectConfigurationAction.applyType(DefaultObjectConfigurationAction.java:120)
        at org.gradle.api.internal.plugins.DefaultObjectConfigurationAction.access$200(DefaultObjectConfigurationAction.java:38)
        at org.gradle.api.internal.plugins.DefaultObjectConfigurationAction$3.run(DefaultObjectConfigurationAction.java:86)
        at org.gradle.api.internal.plugins.DefaultObjectConfigurationAction.execute(DefaultObjectConfigurationAction.java:143)
        at org.gradle.api.internal.project.AbstractPluginAware.apply(AbstractPluginAware.java:46)
        at org.gradle.api.internal.project.ProjectScript.apply(ProjectScript.java:34)
        at org.gradle.api.Script$apply$0.callCurrent(Unknown Source)
        at build_9sk7crqolfjf8m0yenkwy63v1.run(/Users/mchalupa/projects/others/spotbugsFailExample/build.gradle:13)
        at org.gradle.groovy.scripts.internal.DefaultScriptRunnerFactory$ScriptRunnerImpl.run(DefaultScriptRunnerFactory.java:90)
        ... 101 more
```

The reason is is that parent class which spotbugs plugin extends [added a new abstract method](https://github.com/gradle/gradle/commit/d3bcd32c0fde3e9c0915876be57e285ec28cdfbe#diff-f28e55955b51f17c7bf91e023e4404ddR109). 