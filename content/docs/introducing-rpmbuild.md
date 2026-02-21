---
title: "Introducing rpmbuild"
weight: 20
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
# bookIcon: ''
---

# Introducing rpmbuild

> [!INFO]
> This document's audience is engineers evaluating rpmbuild as part of their build tooling. As a sample of my work, it's an important document because it offers an introduction to an open-source project, allowing the project a wider use.

## Key concepts

- **Docker**: Linux containers you can run on your existing infrastructure
- **Pants**: A build system that manages dependencies across large codebases
- **RPM**: Red Hat Package Manager. A way to package software for distribution on a machine
- **RPM spec file**: A configuration file for building a specific RPM

## What rpmbuild is

When creating RPMs, it's important to have a consistent build environment and reproducible builds. It also can be useful to be able to build RPMs in a virtual environment, so you don't need a dedicated build machine.

rpmbuild bridges the gap between an RPM spec file and a working RPM when you otherwise don't have a place to build RPMs. It customizes a pants task for this job. This enables you to check an RPM spec file and pants target into git, enabling reproducible builds.

## Why use rpmbuild

rpmbuild is useful because it allows you more operational control over the RPM build process. With git and pants, you can ensure that a specific RPM was built with a specific version of an RPM spec file. You can manage build artifacts in version control. You also don't need to maintain a build machine on which to build RPMs. Instead, you can use Docker containers to ensure consistent builds.

## How rpmbuild works

rpmbuild defines a pants task for building RPMs given an RPM spec file and references to the file or files used externally. A pants BUILD file makes these references explicit. The task itself wraps Docker, with several default platforms supported.

## Example

For example, this is an rpm spec added to a pants BUILD file:

```python
rpm_spec(
  name='golang',
  spec='golang.spec',
  remote_sources=[
    'https://storage.googleapis.com/golang/go1.7.3.linux-amd64.tar.gz',
  ],
)
```

This is an RPM spec file that installs golang:

```spec
# Disable the debug package (and the associated requirements finding) because RPM gets
# confused by prebuilt Go binaries.
%global debug_package %{nil}

Summary: Go language
Name: golang
Version: 1.7.3
Release: 1%{?dist}
License: GO
Group: Development/Languages
URL: https://golang.org/

BuildRequires: tar

# Disable RPM's standard automatic dependency detection since the Go binaries are static.
AutoReqProv: no

Source: go%{version}.linux-amd64.tar.gz

BuildRoot: %{_tmppath}/%{name}-%{version}-%{release}-root-%(%{__id_u} -n)

%description
Go is an open source programming language that makes it easy to build simple, reliable,
and efficient software. 

%prep
# Do not do any standard source unpacking (-T) and make work directory (-c).
%setup -c -T

%build
# No build step since we are just moving files into the RPM_BUILD_ROOT.

%install
rm -rf "%{buildroot}"
mkdir -p "%{buildroot}/usr/local"
tar xzvf "${RPM_SOURCE_DIR}/go%{version}.linux-amd64.tar.gz" -C "%{buildroot}/usr/local"

mkdir -p "%{buildroot}/usr/local/bin"
cd "%{buildroot}/usr/local/bin"
for x in go godoc gofmt ; do
  ln -s "../go/bin/$x" .
done

%files
%defattr(-, root, root, -)
/usr/local/go
/usr/local/bin/go
/usr/local/bin/godoc
/usr/local/bin/gofmt
```
