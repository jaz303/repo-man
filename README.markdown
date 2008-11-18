repo-man
========

(c) 2008 Jason Frame [jason@onehackoranother.com]

What is it?
-----------

One of the most tedious things about hosting Subversion repositories over HTTP is that staying organised requires frequent editing of your Apache configuration. At work we use a directory structure similar to that show below (repositories are denoted by [R]):

    /
      repo/
        internal/
          project-1/ [R]
          project-2/ [R]
          project-/3 [R]
        client/
          client-1/
            project-4/ [R]
            project-5/ [R]
          client-2/
            project-6/ [R]
            project-7/ [R]
            
The repository roots in the example above are /repo/{internal,client/{client-1,client-2}}, and each requires its own `<Location>` block and `SVNParentPath` directive within your `httpd.conf`. Access control configuration may also be required. As the number of repository roots increases, maintenance becomes increasingly depressing.
  
Enter `repo-man`. It's a (very) simple Ruby tool capable of generating Apache/SVN config fragments for multiple repository roots.


Installation
------------

    sudo gem install jaz303-repo-man
    
Usage
-----

    repo-man path-to-your-repository-root
    
This will spit out Apache configuration code suitable for pasting or dynamic inclusion into a `VirtualHost` block. The path you supply is assumed to correspond to the target virtual host's `DocumentRoot` and the repository locations will be normalised accordingly.

If any of your repositories has specific access control requirements put them in a file called `svn_access` in the corresponding root, or any of its ancestor directories, and `repo-man` will import this using the `AuthzSVNAccessFile` directive. The format of this file is explained at [http://svnbook.red-bean.com/en/1.1/ch06s04.html](), about 2/3 of the way down.