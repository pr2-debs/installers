#! /usr/bin/env python

"""
usage: %(progname)s device
"""


import os, sys, string, time, getopt
import subprocess

import tempfile

def create_usb(device):
  dev1 = device + "1"

  tpath = tempfile.mkdtemp()
  mdev1 = os.path.join(tpath, "dev1")

  subprocess.call(['umount', device])
  for i in range(4):
    subprocess.call(['umount', device + str(i)])

  subprocess.call(['parted', device, '-s', 'mklabel', 'msdos'])

  if 1:
    rc = subprocess.call(['parted', device, 'mkpart', '-s', 'primary', 'fat32', '0', '7700'])
    if rc:
      print "cannot make partition 1"
      return

    subprocess.call(['parted', device, '-s', 'set', '1', 'boot', 'on'])

#  subprocess.call(['dd', 'if=/dev/zero', 'of=' + dev1, 'bs=512', 'count=1'])

  subprocess.call(['mkdosfs', dev1])
  subprocess.call(['syslinux', dev1])


  print "*********** Copying dev1"

  os.mkdir(mdev1, 0755)
  if subprocess.call(['mount', dev1, mdev1]): 
    print "cannot mount dev1"
    return
  os.system('cp -r install_dev1/* %s' % mdev1)
  subprocess.call(["umount", dev1])
  #os.rmdir('dev1')

  os.system('rm -rf %s' % tpath)


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

  device = args[0]

  if subprocess.call(["./verifyinstaller"]) != 0:
    sys.exit(1)

  create_usb(device)

if __name__ == "__main__":
  main(sys.argv, sys.stdout, os.environ)
