# No github release workflow?

This project generates Java POJO classes from XML schema which are not part of 
the source code of this project. Therefore, this project does not contain
any code which could be build automatically by GitHub action, so there is only
a `.github` workflows which checks for gradle being able to start up.

Releases require local schema which are not included due to licence reasons, so
there is no automated `.github` release workflow! 
