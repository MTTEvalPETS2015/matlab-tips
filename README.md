# matlab-tips

### Userful links
#### Stackoverflow
[How can I generate a list of function dependencies in MATLAB?](http://stackoverflow.com/questions/95760/how-can-i-generate-a-list-of-function-dependencies-in-matlab)

> 1. use the built in functions:
    > - mlint
    > - dependency report and
    > - coverage report

> 1. Use Matlab's profiler
  ```matlab
  >> profile on   % turn profiling on
  >> foo;         % entry point to your matlab function or script
  >> profile off  % turn profiling off
  >> profview     % view the report
  ```

> 1. If profiler is not available, then perhaps the following two functions are:
    > - depfun (NOTE: depfun has been removed. Use matlab.codetools.requiredFilesAndProducts instead. See BELOW)
    > - depdir
    
    
[Automatically generating a diagram of function calls in MATLAB [closed]](http://stackoverflow.com/questions/5518200/automatically-generating-a-diagram-of-function-calls-in-matlab)

1. convert deps to .dot then visualise it using GraphViz 
  ```matlab
  function createFunctionDependencyDotFile(calls)
  %CREATEFUNCTIONDEPENDENCYDOTFILE Create a GraphViz DOT diagram file from function call list
  %
  % Calls (cellstr) is an n-by-2 cell array in format {caller,callee;...}.
  %
  % Example:
  % calls = { 'foo','X'; 'bar','Y'; 'foo','Z'; 'foo','bar'; 'bar','bar'};
  % createFunctionDependencyDotFile(calls)
  
  baseName = 'functionCalls';
  dotFile = [baseName '.dot'];
  fid = fopen(dotFile, 'w');
  fprintf(fid, 'digraph G {\n');
  for i = 1:size(calls,1)
      [parent,child] = calls{i,:};
      fprintf(fid, '   "%s" -> "%s"\n', parent, child);
  end
  fprintf(fid, '}\n');
  fclose(fid);
  
  % Render to image
  imageFile = [baseName '.png'];
  % Assumes the GraphViz bin dir is on the path; if not, use full path to dot.exe
  cmd = sprintf('dot -Tpng -Gsize="2,2" "%s" -o"%s"', dotFile, imageFile);
  system(cmd);
  fprintf('Wrote to %s\n', imageFile);
  ```
1. test

### Identify Program Dependencies
#### Simple Display of Program File Dependencies
> [mfiles, mexfiles] = inmem


#### Detailed Display of Program File Dependencies
> use the matlab.codetools.requiredFilesAndProducts function

#### Dependencies Within a Folder

