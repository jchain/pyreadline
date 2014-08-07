pyreadline
==========

an attempt to restore support for ironpython.

c.scroll() and c.scroll_window() are not implemented in IronPython which makes IPython
session very unstable, like dumping continuously the trackback information. So I disable
it temporarily. Another way is to disable the exception throw in c.scroll() instead of
commenting out here.

    x, y = c.pos()       #Preserve one line for Asian IME(Input Method Editor) statusbar
    w, h = c.size()
    if (y >= h - 1) or (n > 0):
        c.scroll_window(-1)
        c.scroll((0, 0, w, h), 0, -1)
        n += 1

NOTE: To implement the console window scrolling in IronPython you probably needs to call
the Win32 function ScrollConsoleWindowBuffer() defined in kernel32.dll. IronPython 2.6 and
above now supports ctypes that is helpful to implement this feature. Similar code is
already there in the file console.py for CPython. It may serve a good start to implement
for IronPython.

A tiny example showing how to call kernel32 api:

    import ctypes
    buffer = ctypes.create_string_buffer(100)
    ctypes.windll.kernel32.GetWindowsDirectoryA(buffer, len(buffer))
    print buffer.value

<!-- vim: set fdm=expr ft=pandoc sw=4 ts=4 tw=0 : -->
