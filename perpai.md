# Getting Started with Porting ev3dev to Debian Trixie

Based on your conversation with the ev3dev maintainer and your goal to help port ev3dev from Debian Stretch to Trixie, here's a comprehensive guide to get you started as a beginner without Debian packaging experience.

## Understanding the Current Situation

The ev3dev project is currently stuck on Debian Stretch (oldoldstable), while Debian Trixie is expected to become the next stable release in August 2025[1]. The maintainer correctly identified that targeting Trixie makes sense for longevity, as it will remain supported longer than targeting intermediate releases.

## Key Components You'll Be Working With

### 1. Package Archive Structure
The ev3dev project maintains its own package archive at archive.ev3dev.org with packages for different Debian distributions[2]. Currently, this includes:
- Packages for jessie, stretch, buster, bullseye distributions
- Both main Debian packages and ev3dev-specific modifications
- Architecture support for armel, armhf, amd64, i386

### 2. Docker Infrastructure  
The project uses Docker extensively for building and distributing packages[3][4]:
- **docker-library**: Creates official ev3dev Docker images following the pattern `ev3dev---`
- **docker-cross**: Provides cross-compilation environments
- **brickstrap**: Converts Docker images into bootable SD card images[5]

## Path and Contribution Strategy

### Phase 1: Set Up Development Environment (Weeks 1-2)

**Install Essential Tools**[6][7]:
```bash
sudo apt-get install build-essential devscripts debhelper dh-make 
sudo apt-get install pbuilder lintian git
```

**Set up a Debian Trixie environment** for testing. You can either:
- Use a virtual machine with Debian Trixie
- Set up a chroot environment with debootstrap[8]
- Upgrade an existing Debian Bookworm system to Trixie[9][10]

### Phase 2: Learn Debian Packaging Basics (Weeks 3-4)

**Study these essential resources**:
1. **Debian New Maintainer's Guide**[7][11] - Start here for packaging fundamentals
2. **Guide for Debian Maintainers** - More comprehensive modern guide  
3. **Basic Debian Packaging Tutorial**[12][13] - Hands-on practice

**Practice with simple packages**:
- Start by creating `.deb` packages for simple scripts or data files
- Learn the structure: `debian/control`, `debian/changelog`, `debian/rules`
- Use `lintian` to check package quality[14]

### Phase 3: Understand ev3dev-Specific Packaging (Weeks 5-6)

**Study existing packages**:
- Browse the ev3dev GitHub organization[15][16] to understand the project structure
- Look at existing package definitions in ev3dev repositories
- Examine how ev3dev modifies upstream Debian packages

**Key repositories to explore**:
- ev3dev package repositories
- Docker image definitions[3]
- Kernel build scripts[17]

### Phase 4: Start Contributing (Week 7+)

**Begin with low-risk packages**:
1. **Start with architecture-independent packages** (`arch: all`) like:
   - Documentation packages
   - Python libraries that don't need compilation
   - Configuration files and scripts

2. **Choose packages that**:
   - Have minimal dependencies
   - Are already working on newer Debian versions
   - Don't require kernel modifications

**Suggested workflow for each package**:

1. **Research the package**:
   - Check if it already exists in Debian Trixie
   - Identify dependencies and their availability in Trixie
   - Review the package's changelog for recent changes

2. **Create/modify packaging**:
   - Update `debian/control` with Trixie-compatible dependencies
   - Modify version numbers appropriately
   - Test building in a clean environment

3. **Test thoroughly**:
   - Build the package in a Trixie environment
   - Test functionality on actual EV3 hardware (if available)
   - Check for regressions compared to Stretch version

4. **Submit for review**:
   - Work with the ev3dev maintainers through GitHub
   - Document changes and testing performed

## Understanding Docker Integration

The maintainer mentioned adding "a new trixie target to https://github.com/ev3dev/docker-library/"[3]. This involves:

1. **Creating new Docker image definitions** for Trixie-based ev3dev images
2. **Updating build scripts** to support the new distribution
3. **Ensuring brickstrap compatibility** with Trixie-based images

This Docker integration is crucial because it's how end users will actually create bootable ev3dev images.

## Recommended Starting Packages

**Good candidates for your first contributions**:

1. **Python packages** - These often port easily if their dependencies are available
2. **Pure data packages** - Themes, documentation, configuration files
3. **Simple utilities** - Small command-line tools with minimal dependencies
4. **Language bindings** - ev3dev libraries for different programming languages

**Packages to avoid initially**:
- Kernel modules or drivers
- Packages requiring cross-compilation setup
- Core system packages with complex dependencies

## Getting Community Support

**Connect with the ev3dev community**:
- **GitHub Issues/Discussions**: Main communication channel[15]
- **Documentation**: Start with ev3dev.org documentation[18]
- **Debian Mentors**: For general Debian packaging questions[19][20]

**Communication approach**:
- Document your progress and share updates
- Ask specific technical questions rather than general ones
- Share your testing results and any issues encountered

## Timeline and Expectations

**Realistic timeline**:
- **Month 1-2**: Learn packaging basics and set up environment
- **Month 3-4**: Start contributing simple packages  
- **Month 5-6**: Handle more complex packages and help with Docker integration
- **Month 6+**: Become a regular contributor helping with ongoing maintenance

The key is starting small and building expertise gradually. Your goal of "turning talks into reality" is achievable, but it requires patience and systematic learning of the packaging ecosystem.

The ev3dev community will likely be very supportive of your efforts, as this is a critical need for the project's future. Focus on learning the fundamentals well, and you'll be able to make meaningful contributions to this important migration effort.

[1] https://release.debian.org/trixie/freeze_policy.html
[2] https://wiki.debian.org/Derivatives/Census/ev3dev
[3] https://github.com/ev3dev/docker-library
[4] https://github.com/ev3dev/docker-cross
[5] https://github.com/ev3dev/brickstrap
[6] https://www.ev3dev.org/docs/devtools/packaging-for-ev3dev/
[7] https://www.debian.org/doc/manuals/debian-faq/contributing.en.html
[8] https://blog.entek.org.uk/technology/2020/06/06/building-debian-vms-with-debootstrap.html
[9] https://fullmetalbrackets.com/blog/upgrade-debian-12-bookworm-debian-13-trixie/
[10] https://gist.github.com/yorickdowne/3cecc7b424ce241b173510e36754af47
[11] https://lwn.net/Articles/987548/
[12] https://www.baeldung.com/linux/create-debian-package
[13] https://askubuntu.com/questions/1345/what-is-the-simplest-debian-packaging-guide
[14] https://moldstud.com/articles/p-how-can-i-contribute-to-debian-as-a-developer
[15] https://github.com/ev3dev
[16] https://github.com/orgs/ev3dev/repositories
[17] https://pkg.go.dev/github.com/ev3go/ev3dev
[18] https://wiki.debian.org/PortsDocs/New
[19] https://www.youtube.com/watch?v=fr_5n2hJ2eU
[20] https://www.reddit.com/r/termux/comments/17mugy0/how_to_deb_port_pkgs_to_termux/
[21] http://docs.ev3dev.org/projects/lego-linux-drivers/en/ev3dev-stretch/ports.html
[22] https://www.ev3dev.org/docs/devtools/installing-the-ev3dev-archive/
[23] https://www.iodigital.com/en/history/intracto/creating-debianubuntu-deb-packages
[24] https://bricks.stackexchange.com/questions/17155/how-to-cross-compile-for-ev3dev-using-docker
[25] https://www.ev3dev.org/docs/tutorials/upgrading-ev3dev/
[26] https://github.com/ev3dev/ev3dev/issues/1418
[27] https://www.ev3dev.org/docs/networking/
[28] https://stackoverflow.com/questions/42940394/ev3-how-to-import-ev3dev-for-python
[29] https://www.debian.org/doc/maint-guide/ch-build.en.html
[30] http://docs.ev3dev.org/projects/lego-linux-drivers/en/ev3dev-stretch/ev3.html
[31] https://ev3dev-lang.readthedocs.io/projects/python-ev3dev/en/stable/faq.html
[32] http://linuxsimba.github.io/building-debian-pkgs
[33] https://packages.debian.org/trixie/amd64/python3-evdev
[34] https://wiki.debian.org/HowToPackageForDebian
[35] http://docs.ev3dev.org/projects/lego-linux-drivers/en/ev3dev-stretch/muxs.html
[36] https://ev3dev-lang.readthedocs.io/projects/python-ev3dev/en/stable/
[37] https://software-dl.ti.com/processor-sdk-linux/esd/AM62X/10_01_10_04_Debian/exports/docs/debian/Building_Debian_Image.html
[38] https://software-dl.ti.com/processor-sdk-linux/esd/AM64X/09_00_00_Debian/exports/docs/debian/Building_Debian_Image.html
[39] https://github.com/ev3dev/docker-base
[40] https://www.ev3dev.org/docs/tutorials/using-brickstrap-to-cross-compile/
[41] https://www.ev3dev.org/docs/tutorials/using-docker-to-cross-compile/
[42] https://www.debian.org/releases/trixie/release-notes/upgrading.en.html
[43] https://www.reddit.com/r/selfhosted/comments/1l6fmt3/debian_users_migration_to_trixie/
[44] https://wiki.debian.org/Debootstrap
[45] https://www.debian.org/releases/trixie/release-notes/issues.en.html
[46] https://www.reddit.com/r/debian/comments/ofty7b/how_to_configure_a_minimalist_debian_on_a_vm_and/
[47] https://wiki.maemo.org/Port_an_existing_Debian_package
[48] https://www.debian.org/doc/developers-reference/pkgs.html
[49] https://github.com/ev3dev/ev3dev/issues/854
[50] https://www.debian.org/ports/sparc/porting
[51] https://www.ev3dev.org
[52] https://github.com/ev3dev/ev3dev/issues/1361
[53] https://www.youtube.com/watch?v=iOadaZaVCok
[54] https://www.ev3dev.org/docs/devtools/setting-up-the-ev3dev-build-ecosystem/
[55] https://www.reddit.com/r/debian/comments/1jkqz9k/what_is_the_cleanest_way_to_install_trixie_right/
[56] https://www.debian.org/doc/manuals/debian-reference/ch02.en.html
[57] https://www.ev3dev.org/projects/
[58] https://unix.stackexchange.com/questions/587801/debian-package-development-process-sid-to-testing-and-back-porting
[59] https://github.com/ev3dev/ev3dev-buildscripts
[60] https://packages.debian.org/bookworm/python3-evdev
[61] https://github.com/ev3dev/rpi-tools
[62] https://github.com/ev3dev/edu-docs/blob/master/docs/getting-started/ev3dev-Repositories.md
[63] http://docs.ev3dev.org/en/ev3dev-stretch/
[64] https://www.reddit.com/r/debian/comments/1gy1lh4/upgrading_debian_system_with_custom_kernel/
[65] https://www.ev3dev.org/projects/2016/08/07/Mapping/
[66] https://packages.debian.org/source/bookworm/python-evdev
[67] https://ev3dev-lang.readthedocs.io/_/downloads/python-ev3dev/en/ev3dev-jessie/pdf/
[68] https://github.com/orgs/ev3dev/discussions
[69] https://ev3dev-lang-java.github.io
[70] https://askubuntu.com/questions/1108793/migration-between-the-different-linux-distributions
[71] https://www.debian.org/doc/manuals/debian-faq/choosing.en.html
[72] https://github.com/ev3dev/ev3dev/issues/1496
[73] https://www.debian.org/doc/manuals/debian-faq/pkg-basics.en.html
[74] https://www.youtube.com/watch?v=Op3lDRRN-Ck
[75] https://www.reddit.com/r/debian/comments/181j1js/migrating_from_distro_to_distro/
[76] https://www.eurobricks.com/forum/forums/topic/168586-ev3dev-venturing-into-the-world-of-ev3dev-and-python/page/4/
[77] https://www.debian.org/doc/manuals/debian-faq/pkgtools.en.html
[78] https://github.com/ev3dev/ev3dev/issues/121
[79] https://ev3lessons.com/en/ProgrammingLessons/beyond-ev3g/EV3Dev-intro.pdf
[80] https://debian-handbook.info/browse/da-DK/stable/sect.how-to-migrate.html
[81] https://debian-handbook.info/browse/stable/sect.becoming-package-maintainer.html
[82] https://www.debian.org/intro/help
[83] https://superuser.com/questions/120760/what-port-needs-to-be-open-for-debian-to-get-updates
[84] https://www.debian.org/doc/maint-guide/start.en.html
[85] https://www.reddit.com/r/debian/comments/1ch260o/contributing_to_debian_how_to_get_started/
[86] https://www.debian.org/ports/
[87] https://www.debian.org/doc/maint-guide/first.en.html
[88] https://www.debian.org/devel/join/
[89] https://serverfault.com/questions/357323/how-can-i-list-my-open-ports-on-debian
[90] https://www.debian.org/doc/maint-guide/
[91] https://www.debian.org/doc/manuals/debmake-doc/
