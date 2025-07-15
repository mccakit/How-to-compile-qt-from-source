=========================
How to Build Qt from Source
=========================

This guide shows how to build Qt from source using `clang` and `Ninja`. The steps below describe how to build the core modules in the correct order.

.. note::
   Do **not** use ``QT_HOST_PATH`` during native builds!

---

Step 1: Build `qtbase`
-----------------------

.. code-block:: bash

   cmake -B build -G Ninja \
     -DCMAKE_C_COMPILER=clang \
     -DCMAKE_CXX_COMPILER=clang++ \
     -DCMAKE_BUILD_TYPE=Release \
     -DQT_BUILD_TOOLS=ON \
     -DQT_BUILD_TESTS=OFF \
     -DQT_BUILD_EXAMPLES=OFF

.. code-block:: bash

   cmake --build build
   cmake --install build --prefix $HOME/dev/qt

---

Step 2: Build `qtshadertools`
-----------------------------

.. warning::
   Do **not** set ``QT_HOST_PATH`` here. It will break the build.

.. code-block:: bash

   cmake -B build -G Ninja ../qtshadertools \
     -DCMAKE_C_COMPILER=clang \
     -DCMAKE_CXX_COMPILER=clang++ \
     -DCMAKE_BUILD_TYPE=Release \
     -DCMAKE_PREFIX_PATH=$HOME/dev/qt \
     -DCMAKE_INSTALL_PREFIX=$HOME/dev/qt \
     -DQT_BUILD_TOOLS=ON \
     -DQT_BUILD_TESTS=OFF \
     -DQT_BUILD_EXAMPLES=OFF \
     -DQt6_DIR=$HOME/dev/qt/lib/cmake/Qt6

.. code-block:: bash

   cmake --build build
   cmake --install build

---

Step 3: Build `qtdeclarative`
-----------------------------

.. code-block:: bash

   cmake -B build -G Ninja ../qtdeclarative \
     -DCMAKE_C_COMPILER=clang \
     -DCMAKE_CXX_COMPILER=clang++ \
     -DCMAKE_BUILD_TYPE=Release \
     -DCMAKE_PREFIX_PATH=$HOME/dev/qt \
     -DCMAKE_INSTALL_PREFIX=$HOME/dev/qt \
     -DQT_BUILD_TOOLS=ON \
     -DQT_BUILD_TESTS=OFF \
     -DQT_BUILD_EXAMPLES=OFF \
     -DQt6_DIR=$HOME/dev/qt/lib/cmake/Qt6

.. code-block:: bash

   cmake --build build
   cmake --install build

---

Step 4: Build `qttools`
------------------------

.. code-block:: bash

   cmake -B build -G Ninja ../qttools \
     -DCMAKE_C_COMPILER=clang \
     -DCMAKE_CXX_COMPILER=clang++ \
     -DCMAKE_BUILD_TYPE=Release \
     -DCMAKE_PREFIX_PATH=$HOME/dev/qt \
     -DCMAKE_INSTALL_PREFIX=$HOME/dev/qt \
     -DQT_BUILD_TOOLS=ON \
     -DQT_BUILD_TESTS=OFF \
     -DQT_BUILD_EXAMPLES=OFF \
     -DQt6_DIR=$HOME/dev/qt/lib/cmake/Qt6

.. code-block:: bash

   cmake --build build
   cmake --install build

---

✅ Now You Have a Native Qt Build
---------------------------------

You can now use your native Qt installation (in `$HOME/dev/qt`) to build apps or cross-compile other versions.

---

🔁 Cross-Compiling Qt (Optional)
--------------------------------

To cross-compile a non-native (e.g., static/shared or for embedded targets), you may set:

- ``QT_HOST_PATH`` to your native Qt install (e.g., ``$HOME/dev/qt``)
- ``Qt6_DIR`` to the cross-build tree

Follow the same module order:

.. code-block::

   qtbase → qtshadertools → qtdeclarative → qttools
