# BD
Base de datos
 CREATE TABLE IF NOT EXISTS public."Especialidad"
(
    "idEspecialidad" integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    "nombreEspecialidad" character varying(50) COLLATE pg_catalog."default" NOT NULL,
    CONSTRAINT "Especialidad_pkey" PRIMARY KEY ("idEspecialidad")
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public."Especialidad"
    OWNER to postgres;

CREATE TABLE IF NOT EXISTS public."EstadoAtencion"
(
    "idEstadoAtencion" integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    "nombreEstadoTicket" character varying(50) COLLATE pg_catalog."default" NOT NULL,
    CONSTRAINT "EstadoAtencion_pkey" PRIMARY KEY ("idEstadoAtencion")
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public."EstadoAtencion"
    OWNER to postgres;

CREATE TABLE IF NOT EXISTS public."RolUsuario"
(
    "idRol" integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    "nombreRolUsuario" character varying(50) COLLATE pg_catalog."default" NOT NULL,
    CONSTRAINT "RolUsuario_pkey" PRIMARY KEY ("idRol")
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public."RolUsuario"
    OWNER to postgres;

CREATE TABLE IF NOT EXISTS public."TipoDocumento"
(
    "idTipoDocumento" integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    "nombreTipoDocumento" character varying(50) COLLATE pg_catalog."default" NOT NULL,
    CONSTRAINT "TipoDocumento_pkey" PRIMARY KEY ("idTipoDocumento")
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public."TipoDocumento"
    OWNER to postgres;

CREATE TABLE IF NOT EXISTS public."Usuarios"
(
    "idUsuario" integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    "idRolUsuario" integer,
    "idTipoEspecialidad" integer,
    correo character varying(100) COLLATE pg_catalog."default",
    contrasena character varying(200) COLLATE pg_catalog."default",
    "cambioContrasena" boolean NOT NULL,
    "idTipoDocumento" integer,
    "nroDocumento" character varying(20) COLLATE pg_catalog."default" NOT NULL,
    nombres character varying(50) COLLATE pg_catalog."default" NOT NULL,
    "apellidoPaterno" character varying(30) COLLATE pg_catalog."default",
    "apellidoMaterno" character varying(30) COLLATE pg_catalog."default",
    CONSTRAINT " Usuarios_pkey" PRIMARY KEY ("idUsuario"),
    CONSTRAINT "fkUsuarioEspecialidad" FOREIGN KEY ("idTipoEspecialidad")
        REFERENCES public."Especialidad" ("idEspecialidad") MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION,
    CONSTRAINT "fkUsuarioRol" FOREIGN KEY ("idRolUsuario")
        REFERENCES public."RolUsuario" ("idRol") MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION,
    CONSTRAINT "fkUsuarioTipoDocumento" FOREIGN KEY ("idTipoDocumento")
        REFERENCES public."TipoDocumento" ("idTipoDocumento") MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
        NOT VALID
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public."Usuarios"
    OWNER to postgres;

CREATE TABLE IF NOT EXISTS public."Solicitud"
(
    "idSolicitud" integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    "idUsuario" integer NOT NULL,
    "idEstadoAtencion" integer NOT NULL,
    "nroSolicitud" character varying(50) COLLATE pg_catalog."default" NOT NULL,
    "fechaHoraSolicitud" timestamp with time zone NOT NULL,
    "tiempoAtencionSolicitud" character varying(20) COLLATE pg_catalog."default",
    CONSTRAINT "Tickets_pkey" PRIMARY KEY ("idSolicitud"),
    CONSTRAINT "fkSolicitudEstado" FOREIGN KEY ("idEstadoAtencion")
        REFERENCES public."EstadoAtencion" ("idEstadoAtencion") MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
        NOT VALID,
    CONSTRAINT "fkSolicitudUsuario" FOREIGN KEY ("idUsuario")
        REFERENCES public."Usuarios" ("idUsuario") MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
        NOT VALID
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public."Solicitud"
    OWNER to postgres;

CREATE TABLE IF NOT EXISTS public."Asignacion"
(
    "idAsignacion" integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    "idUsuarioAtencion" integer,
    "idEstadoAtencion" integer NOT NULL,
    "idTipoEspecialidad" integer NOT NULL,
    "fechaHoraInicioAtencion" timestamp with time zone,
    "fechaHoraFinAtencion" time with time zone,
    motivo character varying COLLATE pg_catalog."default" NOT NULL,
    "idSolicitud" integer,
    "fechaHoraAsignacion" time with time zone,
    CONSTRAINT "DetalleTickets_pkey" PRIMARY KEY ("idAsignacion"),
    CONSTRAINT "fkAsignacionEstado" FOREIGN KEY ("idEstadoAtencion")
        REFERENCES public."EstadoAtencion" ("idEstadoAtencion") MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
        NOT VALID,
    CONSTRAINT "fkAsignacionSolicitud" FOREIGN KEY ("idSolicitud")
        REFERENCES public."Solicitud" ("idSolicitud") MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
        NOT VALID
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public."Asignacion"
    OWNER to postgres;
