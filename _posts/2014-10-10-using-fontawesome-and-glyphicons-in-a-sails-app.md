---
layout: post
title: "Using fontawesome and glyphicons in a sails app"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Place the fonts into the right folder. Add `exportsOverride` to `bower.json`:

    ...
    "exportsOverride": {
      "fontawesome": {
          "css": "css/font-awesome.css",
          "fonts": "fonts/*"
      },
      "bootstrap": {
          "css": ["dist/css/bootstrap.css",
                   "dist/css/bootstrap.css.map"],
          "js": "dist/js/bootstrap.js",
          "fonts": "dist/fonts"
      }
    }
    ...

Add a `fonts` instruction to `tasks/config/copy.js`:

    module.exports = function(grunt) {
    
        grunt.config.set('copy', {
            ...
            fonts: {
                files: [
                    {
                    expand: true,
                    flatten: true,
                    src: ['.tmp/public/vendor/**/fonts/*'],
                    dest: '.tmp/public/fonts'
                }]
            }
        });
    
        grunt.loadNpmTasks('grunt-contrib-copy');
    };

Add `copy:fonts` to `tasks/register/buildProd.js`:

    module.exports = function (grunt) {
        grunt.registerTask('buildProd', [
            'compileAssets',
            'concat',
            'uglify',
            'cssmin',
            'copy:fonts',
            'linkAssetsBuildProd',
            'clean:build',
            'copy:build'
        ]);
    };

Add `copy:fonts` to `tasks/register/prod.js`:

    module.exports = function (grunt) {
        grunt.registerTask('prod', [
            'compileAssets',
            'concat',
            'uglify',
            'cssmin',
            'copy:fonts',
            'sails-linker:prodJs',
            'sails-linker:prodStyles',
            'sails-linker:devTpl',
            'sails-linker:prodJsJade',
            'sails-linker:prodStylesJade',
            'sails-linker:devTplJade'
        ]);
    };
    
    