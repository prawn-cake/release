# Release

Set of recipes and practises how to release projects
 

## Tools

* [git-flow](https://github.com/nvie/gitflow/) - handy git extension
* [md-changelog](https://github.com/prawn-cake/md-changelog) - Tool for managing changelog
* [bumpversion](https://github.com/peritus/bumpversion) - Tool for bumping project version in a single command
* [twine](https://pypi.python.org/pypi/twine) - Tool for uploading python packages to PYPI
* [wheel](http://pythonwheels.com/) - Tool for uploading python packages to PYPI


## Tool configs

**.bumpversion.cfg**
    
    [bumpversion]
    current_version = X.Y.Z
    tag = False
    tag_name = {new_version}
    commit = True
    
    [bumpversion:file:setup.py]
    
    [bumpversion:file:<project>/__init__.py]
    

**md-changelog**
    
    # On project init
    md-changelog init
    

**setup.cfg**

If a project is python{2,3}

    [bdist_wheel]
    universal=1

## One-branch project release
    
    # After commiting latest changes
    
    # Update changelog
    md-changelog feature "Feature X"
    ...
    
    # Update changelog release entry 
    md-changelog release
    
    # Bump version
    bumpversion {patch,minor,major}
    
    # Attach a tag
    git tag -a 1.1.1 -m "Version 1.1.1"
    
    # Push changes with tags
    git push origin master --tags
     
    
## Two-branch project release

Recipe for the projects using master-develop branching model

**IMPORTANT:** Release your project only after all tests are passed 
    
    
    # Init release branch
    git checkout develop
    git flow release start X.Y.Z
    
    # Write necessary changelog entries
    md-changelog bugfix "#25: Fix logging issues"
    md-changelog feature "Feature X"
    md-changelog improvement "Improvement Y"
    
    # Update changelog release entry 
    md-changelog release
    
    # Bump version
    bumpversion {patch,minor,major}
    
    # Finish release
    git flow release finish X.Y.Z
    
    # Checkout to master
    git checkout master
    git push origin master --tags
    
When tests are passed for the master branch

    # Make a wheel
    python setup.py bdist_wheel
    
    # Upload to PYPI
    twine upload dist/<package>-X.Y.Z-py2.py3-none-any.whl -r pypi
