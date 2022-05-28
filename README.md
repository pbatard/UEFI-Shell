UEFI-Shell
==========

[![Build status](https://img.shields.io/github/workflow/status/pbatard/UEFI-Shell/Linux,%20gcc,%20EDK2.svg?label=Build%20Status&style=flat-square)](https://github.com/pbatard/UEFI-Shell/actions/workflows/linux_gcc_edk2.yml)
[![Github stats](https://img.shields.io/github/downloads/pbatard/UEFI-Shell/total.svg?label=Downloads&style=flat-square)](https://github.com/pbatard/UEFI-Shell/releases)
[![Release](https://img.shields.io/badge/Latest%20Release-22H1%20(edk2--stable202205)-blue.svg?style=flat-square)](https://github.com/pbatard/UEFI-Shell/releases)
[![License](https://img.shields.io/badge/License-BSD%202--Clause-orange.svg)](https://opensource.org/licenses/BSD-2-Clause)

This repository contains pre-built UEFI Shell binary images, generated from
official [EDK2](https://github.com/tianocore/edk2) stable releases.

## Usage

These images are provided in the form of a bootable ISO, in order to make them
easy to use with boot media creators such as [Rufus](https://rufus.ie).

However, these can also readily used by:
- Partitioning and formatting a media, such as a USB Flash drive, using a FAT
  file system.
- Extracting the ISO content as is, onto the FAT partition.

Once you have done that, and provided that your machine is set to boot from
removable media (and runs a UEFI firmware that uses one of the architectures
supported by the release), it should automatically boot into the UEFI Shell.

Note that Secure Boot must be disabled for a UEFI Shell media to boot, as
Microsoft does not allow an external UEFI Shell to be signed for Secure Boot.

## Binary validation

These binaries are built in a fully transparent manner, in order to provide
you with complete assurance that they do not contain anything malicious.

To validate that this claim, you can perform the following:

1. Locate the build action for the ISO you downloaded under
   https://github.com/pbatard/UEFI-Shell/actions. For instance, for the 21H1
   release, this would be https://github.com/pbatard/UEFI-Shell/actions/runs/1160237413.
2. Click on `build` to access the build log, and then look at the `Checkout
   repository and submodules` task. The last line for that task provides the
   SHA-1 of the repository commit that was used for the build process (for 21H1
   that would be `19803c2b2183849fc3a4d6f08cc3c0549232df0c`).
3. Append that SHA-1 to `https://github.com/pbatard/UEFI-Shell/commit/` to
   validate that you end up with one of the __public__ commits that were
   pushed to this repository. This validates that the build was not triggered
   by a "hidden" commit, that would perform something malicious, and that we
   would later delete, since it is impossible for anyone without an army of
   supercomputers to alter a git commit in order to "fake" a specific SHA-1.  
   NB: You don't have to take our word for that last claim. Just google "SHA-1
   collision" and also look into the measures that git is taking to switch to
   SHA-256 so as to make the possibility of collision impossible.
4. At this stage, you have assurance that the commit that was used to build
   the binary is a public one. However, you must also further validate that
   the EDK2 source that was used for the build is also the public one that
   is published from https://github.com/tianocore/edk2, and not some private
   potentially malicious copy. To accomplish that, click the `Browse Files`
   button on the page you got from the URL that was constructed above and
   the click on the `edk2 @ #####` link that you see in the repository tree.
   For instance, for 21H1, that link will be labelled `edk2 @ e1999b2`.
5. Validate that this link takes you to a public commit from
   https://github.com/tianocore/edk2. Once you have done that, then you have
   validated that, not only the build cannot have been triggered by a hidden
   commit but also that the EDK2 source for the UEFI Shell that is produced
   by the build cannot have come from anywhere else but the public EDK2
   repository.
6. If you are familiar enough with the build process, you should now look at
   the GitHub actions `.yml` from the commit that was used to trigger the build
   to also validate that it is not doing anything suspiscious (such as
   discarding the built executables to replace them with pre-built malicious
   ones downloaded from a third-party server). Again, because you have already
   validated, with 100% certainty, that all the steps that are used for the
   build can only have come from a public commit which everyone has access to,
   it would simply be impossible for any such behaviour not to appear plainly
   in the `.yml`.
7. At this stage, you should have total confidence that the build process did
   produce binaries that can be __trusted__ to have been built only from the
   public unmodified EDK2 UEFI Shell source. Therefore, the one last item to
   check is to validate that the binaries proposed under this project's Release
   page __are__ the actual binaries that were produced from the build, rather
   than some malicious replacements (since the owner of any GitHub project has
   the ability to delete and replace release files). This last step is very
   easy to accomplish however: As part of the build process, we make sure to
   also display the SHA-256 for all of the UEFI binaries as well as for the
   ISO images being generated.  
   Thus, depending on whether you extracted individual `.efi` files, or are
   working directly with a `.iso`, you can find the relevant SHA-256 displayed
   either under the `Display SHA-256` step or the `Generate ISO images` step
   within the build log (and you should of course have validated that the
   GitHub Actions' `.yml` that was used as part of the build was indeed set
   to perform an actual computation of the SHA-256 from the generated files,
   as opposed to mimicking the display of an SHA-256 computation in order to
   trick someone looking only at the log into thinking that a malicious file
   published under Releases, and that was not generated from the automated
   build process, did come from the build process).
8. Compare the SHA-256 from the build log with the one from the `.efi` or
   `.iso` you downloaded, and verify that they are the same.

If you accomplish all the steps above, then you will have established, with
__absolute__ certainty, that the binaries that are being published on our
Releases page can be trusted not to contain malware (that is, provided you do
accept that toolchains like `gcc` or GitHub employees can be trusted not to
insert malware on their own, but this is outside of the scope of the kind of
assurance that we can provide here).

And the nice thing is that, because any failure of validation for the points we
describe above is __very easy__ to detect, you can rest assured that, even if
you do not go through these steps yourself, someone else is likely to, and is
bound to say something if we ever are to do anything that looks contrary to
our claim that the UEFI Shell binaries published here are 100% trustworthy.
