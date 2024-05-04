<<-----------------------------------------------------------------------------ConfigAndDeployment_Project_03-------------------------------------------------------------------------------------------------->>
# About Project

	- This is a MSBUILD project. The Graphics program renders a graphics scene with the use of models, shaders and a scene description file. The MSBUILD makes sure when the project is build in Release mode, the project will take a copy of the assets and the executable and will zip in a separate package called "project3.zip".
	- The "project3.zip" can be extracted and executed in any location and it'll still render the scene.
	- The file edited for the project is "RenderGraphicsScene.vcxproj" which is in the Project directory. 

## How to build and run : 
	- The solution "Graphics_Project_01.sln" has to be built(in release platform mode), first the static library "MaxGameEngine" project is to be built and then afterwards the "RenderGraphicsScene" project is to be built. 
	- Once succeeded, the zip folder will be created in the solution directory, the exe file will be present in the zip folder. Once extracted it can be executed(probably without any errors!).
	- I have also written text messages for each task and can be viewed in the console. 
	
### Controls(if incase the user wants to explore the rendered scene for fun !) :
	- Default Controls : There are a few controls that I've added :
		- W, A, S, D, Q, E = Move camera Front, Back, Left, Right, Up and Down respectively. 
		- Mouse movement = Camera can be rotated using the mouse
		- Spacebar = Spacebar pauses and resumes the mouse movement, if the user thinks the cam rotation is in the way of examining the scene(By default the cam rotation is in a pause, so when the scene 			loads, press Spacebar to activate camera rotation using mouse)
		-  1, 2, 3, 4 = These numbers keys can be pressed to showcase 4 different angles of the scene.

	- Extra Controls : There a few controls that enable the user to manipulate the models in the scene :
		- CTRL + W, A, S, D, Q, E = To move the models around, Front, Back, Left, Right, Up and Down respectively.
		- CTRL + C, V = To scale the model down or up respectively.
		- CTRL + Z, X = Shift to previous model or next model respectively.
		- CTRL + B = Turn on/off wireframe mode for the selected model.

###Listing the properties, items, tasks and targets

--> Properties :
	1. <DeploymentPath>$(ProjectDir)Deployment</DeploymentPath> 
    	2. <AssetsFolder>$(ProjectDir)Deployment/Assets</AssetsFolder>
    	3. <ModelsFolder>$(DeploymentPath)/Assets/Models</ModelsFolder>
     	4. <ShadersFolder>$(DeploymentPath)/Assets/Shaders</ShadersFolder>

--> Items :
	1. <SceneDescriptionCopyFile Include="SceneDescription.json" />
    	2. <ModelsCopyFiles Include="Assets\Models\*" /> 
    	3. <ShadersCopyFiles Include="Assets\Shaders\*" />
    	4. <ExeFile Include=" ..\x64\$(Configuration)\RenderGraphicsScene.exe" />

--> Tasks :
	1. <MakeDir Directories="$(DeploymentPath);$(AssetsFolder);$(ModelsFolder);$(ShadersFolder)" />
    	2. <Copy sourceFiles="@(SceneDescriptionCopyFile)" DestinationFolder="$(DeploymentPath)" />
    	3. <Copy sourceFiles="@(ModelsCopyFiles)" DestinationFolder="$(ModelsFolder)" />
    	4. <Copy sourceFiles="@(ShadersCopyFiles)" DestinationFolder="$(ShadersFolder)" />
    	5. <Copy sourceFiles="@(ExeFile)" DestinationFolder="$(DeploymentPath)" />
  	6. <Message Text="||MESSAGE|| Zipping the files together" Importance="high" />
    	7. <Exec Command="mkdir &quot;$(ResultFolderPath)&quot;" IgnoreExitCode="true" />
    	8. <Exec Command="&quot;$(ToolPath)7z.exe&quot; a -r &quot;$(ResultFolderPath)project3.zip&quot; &quot;$(DeploymentPath)\*.*&quot;" />
---> Targets :
	1. <Target Name="Checkpoint" AfterTargets="Build" Condition=" '$(Configuration)' == 'Release'">
	2. <Target Name="AfterBuild" DependsOnTargets="Checkpoint" Condition=" '$(Configuration)' == 'Release'">

###“DependsOnTargets” and “Conditional Target”

---> DependsOnTargets :
	- I have made a dependency between two targets mentioned in the above targets section.
---> Conditional Target:
	- I have made a condition on both the targets that I have created to run only in release mode