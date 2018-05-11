# -*- coding: utf-8 -*- #

#                                                          .;;,
#                                     .,.               .,;;;;;,
#                                    ;;;;;;;,,        ,;;%%%%%;;
#                                     `;;;%%%%;;,.  ,;;%%;;%%%;;
#                                       `;%%;;%%%;;,;;%%%%%%%;;'
#                                         `;;%%;;%:,;%%%%%;;%%;;,
#                                            `;;%%%,;%%%%%%%%%;;;
#                                               `;:%%%%%%;;%%;;;'
#           ..,,,.                                 .:::::::.
#        .;;;;;;;;;;,.                                  s.
#        `;;;;;;;;;;;;;,                               ,SSSs.
#          `:.:.:.:.:.:.:.                            ,SSSSSSs.
#           .;;;;;;;;;;;;;::,                        ,SSSSSSSSS,
#          ;;;;;;;;;;;;;;;;:::%,                    ,SS%;SSSSSSsS
#         ;;;;;;,:,:::::::;::::%%,                  SS%;SSSSSSsSS
#         ;;;;;,::a@@@@@@a::%%%%%%%,.   ...         SS%;SSSSSSSS'
#         `::::::@@@@@@@@@@@a;%%%%%%%%%'  #;        `SS%;SSSSS'
#  .,sSSSSs;%%,::@@@@@@;;' #`@@a;%%%%%'   ,'          `S%;SS'
#sSSSSSSSSSs;%%%,:@@@@a;;   .@@@a;%%sSSSS'           .%%%;SS,
#`SSSSSSSSSSSs;%%%,:@@@a;;;;@@@;%%sSSSS'        ..,,%%%;SSSSSSs.
#  `SSSSSSSSSSSSs;%%,%%%%%%%%%%%SSSS'     ..,,%;sSSS;%;SSSSSSSSs.
#     `SSSSSSSSSSS%;%;sSSSS;""""   ..,,%sSSSS;;SSSS%%%;SSSSSSSSSS.
#         """""" %%;SSSSSS;;%..,,sSSS;%SSSSS;%;%%%;%%%%;SSSSSS;SSS.
#                `;SSSSS;;%%%%%;SSSS;%%;%;%;sSSS;%%%%%%%;SSSSSS;SSS
#                 ;SSS;;%%%%%%%%;%;%sSSSS%;SSS;%%%%%%%%%;SSSSSS;SSS
#                 `S;;%%%%%%%%%%%%%SSSSS;%%%;%%%%%%%%%%%;SSSSSS;SSS
#                  ;SS;%%%%%%%%%%%%;%;%;%%;%%%%%%%%%%%%;SSSSSS;SSS'
#                  SS;%%%%%%%%%%%%%%%%%%%;%%%%%%%%%%%;SSSSSS;SSS'
#                  SS;%%%%%%%%%%%%%%%%%%;%%%%%%%%%%%;SSSSS;SSS'
#                  SS;%%%%%%%%%%%%%;sSSs;%%%%%%%%;SSSSSS;SSSS
#                  `SS;%%%%%%%%%%%%%%;SS;%%%%%%;SSSSSS;SSSS'
#                   `S;%%%%%%%%%%%%%%%;S;%%%%%;SSSS;SSSSS%
#                    `S;%;%%%%%%%%%%%'   `%%%%;SSS;SSSSSS%.
#                  ,S;%%%%%%%%%%;'      `%%%%%;S   `SSSSs%,.
#                  ,%%%%%%%%%%;%;'         `%;%%%;     `SSSs;%%,.
#               ,%%%%%%;;;%;;%;'           .%%;%%%       `SSSSs;%%.
#            ,%%%%%' .%;%;%;'             ,%%;%%%'         `SSSS;%%
#          ,%%%%'   .%%%%'              ,%%%%%'             `SSs%%'
#        ,%%%%'    .%%%'              ,%%%%'                ,%%%'
#      ,%%%%'     .%%%              ,%%%%'                 ,%%%'
#    ,%%%%'      .%%%'            ,%%%%'                  ,%%%'
#  ,%%%%'        %%%%           ,%%%'                    ,%%%%
#  %%%%'       .:::::         ,%%%'                      %%%%'
#.:::::        :::::'       ,%%%'                       ,%%%%
#:::::'                   ,%%%%'                        %%%%%
#                        %%%%%'                         %%%%%
#                      .::::::                        .::::::
#                      ::::::'                        ::::::'

import os
import sys

from fabric.api import hide
from fabric.api import lcd
from fabric.api import local



####################################################################
####################################################################


def print_title(s):
    print "#############################################"
    print "#############################################"
    print s


####################################################################
####################################################################

def upgrade(version):
    """Обновит версию LibreOffice. Удалит старую и установит новую."""
    PKG = 'libreoffice'
    TMP_DIR = '/tmp/' + PKG + '_обновить'
    GET = 'wget --verbose --continue --show-progress --directory-prefix=' + TMP_DIR

    URL = 'http://download.documentfoundation.org/' \
        + PKG + '/stable/' + version + '/deb/x86_64/LibreOffice_' \
        + version +'_Linux_x86-64_deb'
    URL_main = URL + '.tar.gz'
    URL_lang = URL + '_langpack_ru.tar.gz'
    URL_help = URL + '_helppack_ru.tar.gz'

    with hide('running'):
        print_title("Создаём новую папку временную:")
        local('rm --force --recursive ' + TMP_DIR)
        local('mkdir --parents ' + TMP_DIR)

    with hide('running'):
        print_title("Смотрим какая установлена текущая версия:")
        local('apt search ^%s[0-9].* |grep установлен' %PKG)
        print_title("Удалим текущую версию:")
        local('sudo apt remove "%s[0-9]*"' %PKG)

    print_title("Загружаем свежую версию во временную папку:")
    local('%s %s' % (GET, URL_main))
    local('%s %s' % (GET, URL_help))
    local('%s %s' % (GET, URL_lang))

    with hide('running'):
        print_title("Идём в папку и распаковываем:")
        with lcd(TMP_DIR):
            local('cat *.tar.gz | tar -xvzf - --ignore-zeros')

            print_title("Устанавливаем новыую версию:")
            DIR_main = '*deb'
            DIR_lang = '*_langpack_ru'
            DIR_help = '*_helppack_ru'

            local('sudo dpkg --install --recursive ./%s/DEBS/*' %DIR_main)
            local('sudo dpkg --install --recursive ./%s/DEBS/*' %DIR_lang)
            local('sudo dpkg --install --recursive ./%s/DEBS/*' %DIR_help)

    with hide('running'):
        print_title("Теперь можно удалить временную папку с загрузками:")
        local('rm --force --recursive ' + TMP_DIR)
        print_title("У нас установлена самая хорошая новая версия программы:")
        local('apt search ^%s[0-9].* |grep установлен' %PKG)
