#! /usr/bin/env python

"""
usage: %(progname)s isofile
"""


import os, sys, string, time, getopt
import subprocess

import tempfile

def create_iso(isofile):

  tpath = tempfile.mkdtemp()

  subprocess.call(['genisoimage', '-R', '-o', isofile, '-b', 'isolinux/isolinux.bin', '-c', 'isolinux/boot.cat', '-no-emul-boot', '-boot-load-size', '4', '-boot-info-table', 'install_dev1'])


def test():
  pass

def usage(progname):
  print __doc__ % vars()

def main(argv, stdout, environ):
  progname = argv[0]
  optlist, args = getopt.getopt(argv[1:], "", ["help", "test", "debug"])

  testflag = 0
  if len(args) == 0:
    usage(progname)
    return
  for (field, val) in optlist:
    if field == "--help":
      usage(progname)
      return
    elif field == "--debug":
      debugfull()
    elif field == "--test":
      testflag = 1

  if testflag:
    test()
    return

  isofile = args[0]

  if subprocess.call(["./verifyinstaller"]) != 0:
    sys.exit(1)

  create_iso(isofile)

if __name__ == "__main__":
  main(sys.argv, sys.stdout, os.environ)
