# Scipy on Big Sur

link: https://scipy.org/install.html

ISSUE: couldn’t do a `pip install scipy` on Mac Big Sur

Read the error message, then did:

    brew install openblas lapack

Still didn’t work. Found this thread: https://github.com/scipy/scipy/issues/13102

Went here: https://pypi.org/project/scipy/#files

Downloaded latest Mac

Renamed it per https://github.com/scipy/scipy/issues/13102

    mv scipy-1.6.0-cp37-cp37m-macosx_10_9_x86_64.whl scipy-1.6.0-cp37-cp37m-macosx_11_0_x86_64.whl

Then did (from within my pyenv virtualenv):

    pip install ~/Downloads/scipy-1.6.0-cp37-cp37m-macosx_11_0_x86_64.whl

Basically this comment: https://github.com/scipy/scipy/issues/13102#issuecomment-733988544
