project('qt4 and 5 build test', 'cpp')

foreach qt : ['qt4', 'qt5']
  qtdep = dependency(qt, modules : ['Core', 'Gui', 'Widgets'], required : qt == 'qt5')
  if qtdep.found()
    qtmodule = import(qt)

    # The following has two resource files because having two in one target
    # requires you to do it properly or you get linker symbol clashes.

    prep = qtmodule.preprocess(
      moc_headers : ['mainWindow.h'],           # These need to be fed through the moc tool before use. 
      ui_files : 'mainWindow.ui',               # XML files that need to be compiled with the uic tol.
      qresources : ['stuff.qrc', 'stuff2.qrc'], # Resource file for rcc compiler.
    )

    qexe = executable(qt + 'app',
      sources : ['main.cpp', 'mainWindow.cpp', # Sources that don't need preprocessing.
      prep],
      dependencies : qtdep)

    # We need a console test application because some test environments
    # do not have an X server.

    qtcore = dependency(qt, modules : 'Core')

    qtcoreapp = executable(qt + 'core', 'q5core.cpp',
      dependencies : qtcore)

    test(qt + 'test', qtcoreapp)

    # The build system needs to include the cpp files from
    # headers but the user must manually include moc
    # files from sources.
    manpreprocessed = qtmodule.preprocess(
      moc_sources : 'manualinclude.cpp',
      moc_headers : 'manualinclude.h')

    qtmaninclude = executable(qt + 'maninclude',
      sources : ['manualinclude.cpp', manpreprocessed],
      dependencies : qtcore)

    test(qt + 'maninclude', qtmaninclude)
  endif
endforeach