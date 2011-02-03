# Copyright 2010 - 2011, Qualcomm Innovation Center, Inc.
# 
#    Licensed under the Apache License, Version 2.0 (the "License");
#    you may not use this file except in compliance with the License.
#    You may obtain a copy of the License at
# 
#        http://www.apache.org/licenses/LICENSE-2.0
# 
#    Unless required by applicable law or agreed to in writing, software
#    distributed under the License is distributed on an "AS IS" BASIS,
#    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#    See the License for the specific language governing permissions and
#    limitations under the License.
# 

Import('env')

# Indicate that this SConscript file has been loaded already
env['_ALLJOYNCORE_'] = True

# Dependent Projects
common_hdrs, common_objs = env.SConscript(['../common/SConscript'])
if env['OS'] == 'windows' or env['OS'] == 'android':
    env.SConscript(['../stlport/SConscript'])

# Add support for multiple build targets in the same workset
env.VariantDir('$OBJDIR', 'src', duplicate = 0)
env.VariantDir('$OBJDIR/test', 'test', duplicate = 0)
env.VariantDir('$OBJDIR/daemon', 'daemon', duplicate=0)
env.VariantDir('$OBJDIR/samples', 'samples', duplicate = 0)

# AllJoyn Install
env.Install('$OBJDIR', env.File('src/Status.xml'))
env.Status('$OBJDIR/Status')
env.Install('$DISTDIR/inc', env.File('inc/Status.h'))
env.Install('$DISTDIR/inc/alljoyn', env.Glob('inc/alljoyn/*.h'))
for d,h in common_hdrs.items():
    env.Install('$DISTDIR/inc/%s' % d, h)

# Header file includes
env.Append(CPPPATH = [env.Dir('inc')])

# Make private headers available
env.Append(CPPPATH = [env.Dir('src')])

# AllJoyn Libraries
libs = env.SConscript('$OBJDIR/SConscript', exports = ['common_objs'])
dlibs = env.Install('$DISTDIR/lib', libs)
env.Append(LIBPATH = [env.Dir('$DISTDIR/lib')])
env.Prepend(LIBS = dlibs)

# AllJoyn Daemon
daemon_progs = env.SConscript('$OBJDIR/daemon/SConscript')
env.Install('$DISTDIR/bin', daemon_progs)

# Test programs
progs = env.SConscript('$OBJDIR/test/SConscript')
env.Install('$DISTDIR/bin', progs)

# Sample programs
progs = env.SConscript('$OBJDIR/samples/SConscript')
env.Install('$DISTDIR/bin/samples', progs)

# Release notes and misc.
env.Install('$DISTDIR', 'docs/ReleaseNotes.txt')
env.InstallAs('$DISTDIR/README.txt', 'docs/README.android')
env.Install('$DISTDIR', 'NOTICE')
env.Install('$DISTDIR', 'README.md')

# Build docs
doxy = env.Doxygen('docs/Doxygen')
env.Alias('docs', doxy)

#if the command scons OS=android CPU=arm VARIANT=release ANDRID_NDK=<ndk_location> ANDROID_SRC=<android_source_location> is DOCS=pdf used 
# this will generate the latex files in 'docs/android_release_Doxygen' go into that directory type 'make' to generate the pdf version of
# the output file.  The resulting pdf will have the name refman.pdf (The scons script to generates errors when trying to create 
# the pdf version file that is why the extra step of the make file is needed. )
if env['OS'] == 'android' and env['VARIANT'] == 'release' and env['DOCS'] == 'pdf':
    env.Doxygen('docs/android_release_Doxygen')
    #copy the custom style to the latex folder generated by doxygen.
    Command('./docs/android_release_latex/quic.sty','./docs/quic.sty',Copy("$TARGET", "$SOURCE"))
    #env.PDF('./docs/android_release_latex/refman.pdf', './docs/android_release_latex/refman.tex')
    #Command('./build/android/arm/release/dist/docs/refman.pdf','./docs/android_release_latex/refman.pdf',Copy("$TARGET", "$SOURCE"))
elif env['DOCS'] == 'pdf':
    #copy the custom style to the latex folder generated by doxygen.
    Command('./docs/latex/quic.sty','./docs/quic.sty',Copy("$TARGET", "$SOURCE"))
    

